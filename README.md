# Building / Dependencies
The sample library was built using VS2019 against the C++14 standard. Make sure redistributables are installed

The calling program uses dotnet core 3.1

# Intro

The application consists of a single class that should be extended and an entry point that should use that class.
You can add as many classes as you wish.

The dll file in `libs` is a fake implementation of the dlls we get from our colleagues in Taiwan. We have multiple of these libraries for different devices, they have all the same signatures. The only interesting methods for this test are `GetKeyData` and `SetFunctionCallBack`. Ignore every other function.

# Tasks

## 1. Load Library and Methods
* [Task] Load the dll and both mentioned methods using PInvoke. Do not use the `[DllImport]` attribute for loading this library.
* [Question] Why don't we use the [DllImport] method for these libraries?

## 2. Get key data
The `uint32_t GetKeyData(int pid, int profileNum, int keyNo, int layer)` function returns an unsigned int which represents a KeyAssignment (what a button does) on a device. The documentation of this representation can be found in [keyassignments.png](./keyassignments.png). For this the parameters don't matter - they can be set to 0.

* [Task] Call the `GetKeyData` method on the library and convert the returned uint32_t into a byte array that can be used further.
* [Question] What KeyAssignment does it represent?

## 3. Connection Status
The devices can communicate with our code using Callbacks. One of the use cases is getting a notification when a device connects. To simulate this the `SetFunctionCallBack` will start sending random messages and one which represents a wireless connection.

```cs
// Function call back, when key press or function trigger will call this callback
void SetFunctionCallBack(int pid, FunctionCallBack callBack)
// typedef void(*FunctionCallBack)(int, unsigned char *);   // PID, Information (size of 8)
```

Is the documentation of the function. Again the pid doesn't matter, the callback however does. A connection change is indicated by the info[0] being 0xE6 and info[4] greater than zero

1. [Task] Use the `SetFunctionCallBack` function to register a callback
2. [Task] Print a message when a connection callback was read
3. [Question] What unexpected behaviour can happen when passing the callback?
