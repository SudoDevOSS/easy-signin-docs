# Swift Examples


## Login

``` swift
private var disposables = Set<AnyCancellable>()
// Authenticate user with service
let loginUseCase = DependencyContainer.shared.container.resolve(LoginUseCaseDelegate.self)
loginUseCase
.exec(payload: .init(email: "demo@example.com", password: "SOME_PASSWORD"))
.sink{ value in 
    switch value {
        case .failure(let err):
            // Handle error
            break
        case .finished:
            break
    }    
} receiveValue: { value in 
    // Handle login response
}
.store(in: &disposables)
```

## Register

``` swift
private var disposables = Set<AnyCancellable>()
// Register new user with service
let registerUseCase = DependencyContainer.shared.container.resolve(RegisterUseCaseDelegate.self)
registerUseCase
.exec(payload: .init(firstName: "Demo", lastName: "User", email: "demo@example.com", password: "SOME_PASSWORD", confirmPassword: "SOME_PASSWORD", phoneNumber: "NUM"))
.sink{ value in 
    switch value {
        case .failure(let err):
            // Handle error
            break
        case .finished:
            break
    }    
} receiveValue: { value in 
    // Handle register response
}
.store(in: &disposables)
```

## Reset Password

``` swift
private var disposables = Set<AnyCancellable>()
// Request reset email for user
let resetPasswordUseCase = DependencyContainer.shared.container.resolve(ResetPasswordUseCase.self)
resetPasswordUseCase
.exec(payload: .init(email: "demo@example.com"))
.sink{ value in 
    switch value {
        case .failure(let err):
            // Handle error
            break
        case .finished:
            break
    }    
} receiveValue: { value in 
    // Handle reset password response
}
.store(in: &disposables)
```

## Fetch User Account Info

``` swift
private var disposables = Set<AnyCancellable>()
// Fetch user account info from service
let fetchAccountInfoUseCase = DependencyContainer.shared.container.resolve(FetchUserInfoUseCaseDelegate.self)
fetchAccountInfoUseCase
.exec()
.sink{ value in 
    switch value {
        case .failure(let err):
            // Handle error
            break
        case .finished:
            break
    }    
} receiveValue: { value in 
    // Handle account info response
}
.store(in: &disposables)
```

## Update User Account Info

``` swift
private var disposables = Set<AnyCancellable>()
// Update user account info
let updateInfoUseCase = DependencyContainer.shared.container.resolve(UpdateUserInfoUseCaseDelegate.self)
// ID is optional in-case it is picked up by the server from authentication token
updateInfoUseCase
.exec(payload: .init(id: nil, firstName: "Demo", lastName: "User", email: "demo@example.com", phoneNumber: "NUM"))
.sink{ value in 
    switch value {
        case .failure(let err):
            // Handle error
            break
        case .finished:
            break
    }    
} receiveValue: { value in 
    // Handle update info response
}
.store(in: &disposables)
```