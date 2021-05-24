# Classes

In here you will find description about classes and return types of each use case

## About
This library exposes 7 classes:
- **DepndencyContainer**: Handles injecting delegate implementation when needed
- **BaseNetworkConfigurator**: Configures base url & authentication token for all network calls
- **LoginUseCase**: Authenticate user with service
    - **AuthenticationResponseDto**
- **RegisterUseCase**: Registers new user with service
    - **AuthenticationResponseDto**
- **ResetPasswordUseCase**: Request reset password email for user
- **FetchAccountInfoUseCase**: Fetch user info from service
- **UpdateAccountInfoUseCase**: Update user info in service


## Details

### DependencyContainer
A singleton class handles injecting delegate implementation when needed

**Usage**
``` swift
/** LoginUseCase delegate can be replaced with either of the following:
    
    1. RegisterUseCaseDelegate
    2. ResetPasswordUseCaseDelegate
    3. FetchAccountInfoUseCaseDelegate
    4. UpdateAccountInfoUseCaseDelegate
*/
DepenedencyContainer.shared.container.resolve(LoginUseCaseDelegate.self)
```
### BaseNetworkConfigurator
Handles initializing base url and authentication token for all api calls
``` swift
// Configure network base url & authentication token
// This function needs to be called again when providing token
// at a later stage 
BaseNetworkConfigurator.shared.configure(url: "YOUR_URL", token: "OPTIONAL_AUTH_TOKEN")
```

### LoginUseCaseDelegate
Takes ``` LoginRequestDto ``` as input and returns ``` AnyPublisher<AuthenticationResponseDto, BaseError> ```

New instance can be obtained by calling 
``` swift
 DependencyContainer.shared.container.resolve(LoginUseCaseDelegate.self) 
```

### RegisterUseCaseDelegate
Takes ``` RegisterRequestDto ``` as input and returns ``` AnyPublisher<AuthenticationResponseDto, BaseError> ```

New instance can be obtained by calling 
``` swift
 DependencyContainer.shared.container.resolve(RegisterUseCaseDelegate.self)
```

### ResetPasswordUseCaseDelegate
Takes ``` String ``` as input and returns ``` AnyPublisher<Bool, BaseError> ```

New instance can be obtained by calling 
``` swift
 DependencyContainer.shared.container.resolve(ResetPasswordUseCaseDelegate.self)
```

### FetchUserInfoUseCaseDelegate
Does not require an input and returns ``` AnyPublisher<User, BaseError> ```

New instance can be obtained by calling 
``` swift
 DependencyContainer.shared.container.resolve(FetchUserInfoUseCaseDelegate.self)
```

### UpdateUserInfoUseCaseDelegate
Takes ``` User ``` as input and returns ``` AnyPublisher<User, BaseError> ```

New instance can be obtained by calling 
``` swift
 DependencyContainer.shared.container.resolve(UpdateUserInfoUseCaseDelegate.self)
```

---

## Internal Classes

List of classes to take note of that effect the behavior of use cases

### EmailField

Validates email after setting value, if new value is not valid it falls back to last valid value or default value which is an empty string

``` swift
/**
    Validation is done executing a regex and can be found in: Core -> Extensions -> StringExtensions.swift
*/
public struct EmailField { 
    public private(set) var value: String = ""  {
        didSet(fromOldValue){ 
            if !value.isValidEmail() {
                value = fromOldValue
            }
        }
    }
}
```


## Dto Classes

### LoginRequestDto
Contains payload sent with login request.

Exposes the following props: 
``` swift
public let email: EmailField
public let password: String
```
Can be initialized by calling

``` swift
LoginRequestDto(email: "USER_EMAIL", password: "USER_PASSWORD")
```

### AuthenticationResponseDto
Response from ``` LoginUseCaseDelegate ``` and ``` RegisterUseCaseDelegate ``` exposes the following props

``` swift
public let accessToken: String
public let refreshToken: String
```


### RegisterRequestDto
Contains payload sent with register request

Exposes the following props:

``` swift
public let firstName: String
public let lastName: String
public let password: String
public let confirmPassword: String
public let email: EmailField
public let phoneNumber: String
```

Can be initialized by calling
``` swift
RegisterRequestDto(firstName: "FIRST", lastName: "LAST", email: "EMAIL", password: "PASSWORD", confirmPassword: "PASSWORD", phoneNumber: "PHONE")
```

### User
Returned by ``` FetchUserInfoUseCaseDelegate ``` and also consumed by ``` UpdateUserInfoUseCaseDelegate ```

Exposes the following props
``` swift
public let id: String?
public private(set) var firstName: String
public private(set) var lastName: String
public private(set) var email: EmailField
public private(set) var phoneNumber: String
```

Exposes the following methods: 
``` swift
/**
    Validates and change first & last names if values are valid
    Else nothing is applied
*/
public func changeName(firstName: String? = nil, lastName: String? = nil)

/**
    Validates and change phone number if value is valid
    Else nothing is applied
*/
public func changePhoneNumber(_ newNumber: String)

/**
    Validates and change email if value is valid
    Else nothing is applied
*/
public func changeEmail(_ newNumber: String)
```

Can be initialized by calling
``` swift
User(firstName: "FIRST", lastName: "LAST", email: "EMAIL", phoneNumber: "NUMBER")
```

# Next
[Examples](/swift/examples/?id=swift-examples)