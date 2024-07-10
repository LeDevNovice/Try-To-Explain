# Try to explain... What is the difference between native TypeScript type and mapped type in NestJS ?

### TypeScript Beyond Execution Scope
Types in TypeScript, such as `Partial<T>`, `Pick<T, K>`, and `Omit<T, K>`, are tools for manipulating types at compile time. However, their scope is limited to type checking before the code is executed.

When you write TypeScript code, the compiler checks the types and helps you detect potential errors before your code is executed. Types are instructions for this compiler. For example, `Partial<T>` indicates to the compiler that all properties of type T become optional. This only modifies the way the compiler perceives these types. If you have a User interface and use `Partial<User>`, the compiler knows that it should treat all properties of User as optional.

When TypeScript is transpiled to JavaScript, type information is lost. TypeScript produces plain JavaScript because JavaScript itself has no notion of types. Types have no direct effect on the produced JavaScript code. Since type transformations are only compile-time abstractions, they leave no trace in the executed code. Once transpiled to JavaScript, there is no indication that this type was partially defined.

Therefore, if you need to manipulate types or data structures based on types at runtime, TypeScript’s mapped types will not help, as they leave no trace in the transpiled JavaScript.

### The Difference with NestJS Mapped Types
NestJS offers utility functions like PartialType, PickType, and OmitType to handle type transformations while also manipulating metadata and decorators. This is essential to maintain the integrity and consistency of schemas and validations in applications that use packages like `@nestjs/graphql` or `@nestjs/swagger`.

Consider a UserDto class with `@ApiProperty` decorators from `@nestjs/swagger`:

``` typescript
import { ApiProperty } from '@nestjs/swagger';

class UserDto {
  @ApiProperty()
  id: number;

  @ApiProperty()
  name: string;

  @ApiProperty()
  email: string;
}
```

As mentioned earlier, if you use TypeScript's `Omit<UserDto, 'email'>`, the compiler will understand that the resulting type does not have the email property, but this will not affect the metadata related to the `@ApiProperty` decorators. The transpiled JavaScript code will not contain any indication of the omission of the email property or its metadata. This can lead to non-functional program behavior compared to what we intended to implement.

With NestJS, you can use `OmitType` to achieve the same result while also manipulating the metadata:

``` typescript
import { OmitType } from '@nestjs/mapped-types';

class PartialUserDto extends OmitType(UserDto, ['email']) {}
```

In this case, `OmitType` will not only omit the email property from `UserDto` but also remove the metadata and decorators associated with this property. This ensures that the generated schemas for the APIs and validations remain consistent and correct.

When defining schemas for APIs with `@nestjs/swagger`, it is crucial that the schemas accurately reflect the types used. If you omit a property without manipulating the metadata, your schema can become incorrect, leading to validation errors or inconsistencies in the API documentation. Decorators like `@ApiProperty` add metadata used for validation and schema generation. If you simply use TypeScript's mapped types, these decorators remain attached to the properties even if they are omitted. NestJS utility functions ensure that these decorators are also removed or modified accordingly.