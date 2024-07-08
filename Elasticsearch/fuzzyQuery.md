# Try to explain... What is a Fuzzy Query, how does it works and how to use it ?

_Users often make typos when conducting searches. These errors can prevent Elasticsearch from finding relevant documents in the cluster indices because the misspelled terms do not match the exact terms in the indexed documents. To ensure that searches remain effective despite these potential typos, Elasticsearch uses the concept of "fuzziness"._

_Fuzziness in Elasticsearch broadens the search criteria to include terms similar to those being searched for, taking into account typos or spelling variations. For example, if a user searches for "bonour" instead of "bonjour", fuzziness can enable the search to find documents containing "bonjour" despite the typo. This mechanism significantly improves the accuracy and adaptability of searches._

---

### Implementation
One way to implement the concept of fuzziness in Elasticsearch is to integrate it directly into the search query body using the fuzziness parameter.

``` json
{
    "query": {
        "match": {
            "name": {
                "query": "bonour",
                "fuzziness": "auto"
            }
        }
    }
}
```

With this configuration, the search becomes more tolerant of typos. The fuzziness parameter allows Elasticsearch to insert, delete, or modify certain characters in the query to find matches in the indexed documents.

This mechanism ensures that search results remain relevant and accurate, even in the presence of typographical errors.

### How does it work ?
The concept of fuzziness in Elasticsearch is based on the Levenshtein distance. This algorithm measures the minimum number of edits required to transform one word into another, considering insertions, deletions, or substitutions of characters. For example, to transform "bonour" into "bonjour", only one modification is necessary (adding the missing "j"), so the Levenshtein distance is 1.

When using the fuzziness parameter with the value auto in an Elasticsearch search query, it automatically handles typo tolerance based on the length of the word:

- Terms with 2 characters or fewer: No typo tolerance. The term must match exactly.
  
- Terms with 3 to 5 characters: A Levenshtein distance of 1 is applied. Elasticsearch can modify, add, or delete one character to find matches.
  
- Terms with more than 5 characters: A Levenshtein distance of 2 is applied. Elasticsearch can make up to two edits to find matches.

Studies show that 80% of human typographical errors can be corrected with a Levenshtein distance of 2, which is why this is the maximum used by Elasticsearch. Increasing this distance could compromise search performance by increasing the number of potentially matching terms, thereby diluting precision.

For example, for the term "lobster" which contains 7 characters, two edits are possible. Elasticsearch could delete the "L" and replace the "B" with a "Y" to get the term "oyster", thus returning documents containing this term even if it was not what the user initially searched for.

It's important to note that fuzziness applies to each term in the query individually and not to the entire phrase. Thus, the Levenshtein distance is calculated separately for each word if the user's search consists of a phrase or a set of terms.