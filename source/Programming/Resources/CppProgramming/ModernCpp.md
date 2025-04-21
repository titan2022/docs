# Modern C++

## Smart pointers

Added in C++11, smart pointers completely replace the old asterisk `*` pointer from C. All usage of classic pointers should be replaced with smart pointers as they're safer: their behavior is more specific and conveys correct usage to the programmer more clearly. Restricted freedom also means that it will allow to catch errors quicker with error handling.

### `std::shared_ptr`

This smart pointer should be used in all occasions where ownership and access is intended to be shared between several pointer objects. This means that destroying one instance of a shared pointer pointing to one specific object will not deallocate the object or dereference any other pointers pointing to it. Most typical classic pointer usages can be replaced with shared pointers.

[...]

### `std::unique_ptr`

This smart pointer should be used in all occasions where access is limited to only one pointer object. This means that you cannot create multiple unique pointers referencing the same object (please don't write code that tries to do that, as it will [...]). If you dereference a unique pointer, it will automatically deallocate the data it was pointing to.

[...]

## File IO

### `std::filesystem`