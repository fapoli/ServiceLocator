# MoodyLib.ServiceLocator

A lightweight and convenient way to access shared components across your project without relying on the singleton pattern.  
Useful for small or mid-sized architectures where dependency injection frameworks would be overkill.

## Contents
- **ServiceLocator class** – Provides a simple API for registering and retrieving instances by type.

## Install via Git URL

1. In Unity, open **Window > Package Manager**.
2. Click the **+** button in the top-left corner.
3. Select **Add package from Git URL…**
4. Paste the following URL and click **Add**:

   ```text
   https://github.com/fapoli/MoodyLib.ServiceLocator.git
   ```

Unity will download and install the package. After installation, it will appear under the **Packages** folder.


## How to use

### 1. Register a service

You can register any object instance using:

```csharp
ServiceLocator.Register<IMyService>(new MyService());
```

The service is stored under its type.  
Only **one instance per type** is allowed and new registrations overwrite the previous one.

### 2. Resolve a service

Retrieve the instance from anywhere in your code:

```csharp
var myService = ServiceLocator.Resolve<IMyService>();
```

If no service of that type was registered, `null` (or `default`) is returned.

## Example

```csharp
public interface IAudioService {
    void Play(string soundId);
}

public class AudioService : IAudioService {
    public void Play(string soundId) {
        // Play audio…
    }
}

public class GameInitializer : MonoBehaviour {
    void Awake() {
        ServiceLocator.Register<IAudioService>(new AudioService());
    }
}

public class Enemy : MonoBehaviour {
    void OnDeath() {
        var audio = ServiceLocator.Resolve<IAudioService>();
        audio?.Play("enemy_death");
    }
}
```

## Best Practices

### ✔ Keep it simple
Use the Service Locator for small utility components, managers, and systems that truly need global access.

### ✔ Register services early
The safest place to register services is during initialization (e.g., `Awake()`, bootstrap scene, or startup system).

### ✔ Prefer interfaces
Register by interface instead of concrete type to make code more flexible and testable.

```csharp
ServiceLocator.Register<ILogService>(new UnityLogService());
```

### ✔ Reset between tests
Call `ServiceLocator.Reset()` before or after each test to ensure a clean state.

### ✔ Avoid overusing it
While convenient, the Service Locator is still a form of global state.  
Use it sparingly to avoid hidden dependencies and hard-to-track coupling.


## Method Reference

### `Register<T>(object instance)`
Registers an instance under the type `T`. Overwrites existing registrations.

### `Resolve<T>()`
Returns the registered instance of type `T`, or `null` if none was registered.

### `Reset()`
Clears all registered services.
