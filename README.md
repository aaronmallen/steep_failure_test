# Steep or RBS Collection Error

This demonstrates that steep fails on itself on initial install of rbs collection.

## Steps to reproduce

The commits in this repository demonstrate how to replicate this error:

1. [06f8205](https://github.com/aaronmallen/steep_failure_test/commit/06f8205) Generate a gem with `bundle gem steep_failure_test`
2. [e48e0c7](https://github.com/aaronmallen/steep_failure_test/commit/e48e0c7) install rbs and steep
3. [fceadc0](https://github.com/aaronmallen/steep_failure_test/commit/fceadc0) Run `bundle exec steep init`
4. [a97026f](https://github.com/aaronmallen/steep_failure_test/commit/a97026f) Edit steep target (notice steep check error)
5. [533f29f](https://github.com/aaronmallen/steep_failure_test/commit/533f29f) Add rbs signature `SteepFailureTest::Error` to satisfy the steep check
6. [4d3a879](https://github.com/aaronmallen/steep_failure_test/commit/4d3a879) Run `bundle exec rbs collection init`
7. [e1f9136](https://github.com/aaronmallen/steep_failure_test/commit/e1f9136) Run `bundle exec rbs collection install`
8. run bundle exec steep check and see error

```
   ~/steep_failure_test  on   main ⇡2 ?2 ································································ ✔  took 4s   3.3.4   system   at 18:00:21  ─╮
╰─ bundle exec steep check                                                                                                                                                   ─╯
# Type checking files:

......................................................................................................F.....................................F.........................................2024-11-25 18:01:04.108: ERROR: [Steep 1.8.3] [typecheck:typecheck@6] [background] [#validate_signature(path=/Users/aaron.allen/.asdf/installs/ruby/3.3.4/lib/ruby/gems/3.3.0/gems/ffi-1.17.0-arm64-darwin/sig/ffi/struct.rbs)] [::FFI::Struct] [::FFI::Struct::ManagedStructConverter[::FFI::ManagedStruct[::FFI::AutoPointer, untyped], ::FFI::AutoPointer] <: ::FFI::_DataConverter[::FFI::Pointer, instance, untyped]] [from_native : (::FFI::Pointer, untyped) -> ::FFI::ManagedStruct[::FFI::AutoPointer, untyped] <: (::FFI::Pointer, untyped) -> instance] [::FFI::ManagedStruct[::FFI::AutoPointer, untyped] <: instance] `T <: instance` doesn't hold generally, but testing it with `::FFI::ManagedStruct[::FFI::AutoPointer, untyped] <: instance && instance <: ::FFI::ManagedStruct[::FFI::AutoPointer, untyped]` for compatibility
..2024-11-25 18:01:04.136: ERROR: [Steep 1.8.3] [typecheck:typecheck@6] [background] [#validate_signature(path=/Users/aaron.allen/.asdf/installs/ruby/3.3.4/lib/ruby/gems/3.3.0/gems/ffi-1.17.0-arm64-darwin/sig/ffi/struct.rbs)] [::FFI::StructByReference[::FFI::Struct[::FFI::AbstractMemory, untyped], ::FFI::AbstractMemory] <: ::FFI::_DataConverter[::FFI::AbstractMemory, instance, untyped]] [from_native : (::FFI::AbstractMemory, untyped) -> ::FFI::Struct[::FFI::AbstractMemory, untyped] <: (::FFI::AbstractMemory, untyped) -> instance] [::FFI::Struct[::FFI::AbstractMemory, untyped] <: instance] `T <: instance` doesn't hold generally, but testing it with `::FFI::Struct[::FFI::AbstractMemory, untyped] <: instance && instance <: ::FFI::Struct[::FFI::AbstractMemory, untyped]` for compatibility
...F......................................................................................................................

../../.asdf/installs/ruby/3.3.4/lib/ruby/gems/3.3.0/gems/ffi-1.17.0-arm64-darwin/sig/ffi/library.rbs:36:45: [error] Type `::FFI::DataConverter` is generic but used as a non generic type
│ Diagnostic ID: RBS::InvalidTypeApplication
│
└     def typedef: [T < Type] (T old, Symbol | DataConverter add, ?untyped) -> T
                                               ~~~~~~~~~~~~~

../../.asdf/installs/ruby/3.3.4/lib/ruby/gems/3.3.0/gems/ffi-1.17.0-arm64-darwin/sig/ffi/auto_pointer.rbs:15:65: [error] Type application of `::FFI::AutoPointer::Releaser::_Proc` doesn't satisfy the constraints: self <: ::FFI::Pointer
│   self <: ::FFI::Pointer
│     singleton(::FFI::AutoPointer) <: ::FFI::Pointer
│       singleton(::FFI::Pointer) <: ::FFI::Pointer
│         singleton(::FFI::AbstractMemory) <: ::FFI::Pointer
│           singleton(::Object) <: ::FFI::Pointer
│             singleton(::BasicObject) <: ::FFI::Pointer
│               ::Class <: ::FFI::Pointer
│                 ::Module <: ::FFI::Pointer
│                   ::Object <: ::FFI::Pointer
│
│ Diagnostic ID: RBS::UnsatisfiableTypeApplication
│
└     def initialize: (Pointer pointer, Method | ^(self) -> void | Releaser::_Proc[self] callable) -> self
                                                                   ~~~~~~~~~~~~~~~~~~~~~

../../.asdf/installs/ruby/3.3.4/lib/ruby/gems/3.3.0/gems/ffi-1.17.0-arm64-darwin/sig/ffi/struct.rbs:23:29: [error] Type application of `::FFI::Type::Mapped` doesn't satisfy the constraints: ::FFI::Struct::ManagedStructConverter[::FFI::ManagedStruct[::FFI::AutoPointer, untyped], ::FFI::AutoPointer] <: ::FFI::_DataConverter[::FFI::Pointer, instance, untyped]
│   ::FFI::Struct::ManagedStructConverter[::FFI::ManagedStruct[::FFI::AutoPointer, untyped], ::FFI::AutoPointer] <: ::FFI::_DataConverter[::FFI::Pointer, instance, untyped]
│     #<Steep::Interface::Shape: type=::FFI::Struct::ManagedStructConverter[::FFI::ManagedStruct[::FFI::AutoPointer, untyped], ::FFI::AutoPointer], private?=false, methods={!, !=, !~, <=>, ==, ===, Namespace, TypeName, __id__, __send__, acts_like?, as_json, blank?, class, class_eval, clone, deep_dup, define_singleton_method, display, dup, duplicable?, enum_for, eql?, equal?, extend, freeze, from_native, frozen?, hash, html_safe?, in?, inspect, instance_eval, instance_exec, instance_of?, instance_values, instance_variable_defined?, instance_variable_get, instance_variable_names, instance_variable_set, instance_variables, is_a?, itself, kind_of?, method, methods, native_type, nil?, object_id, presence, presence_in, present?, private_methods, protected_methods, public_method, public_methods, public_send, remove_instance_variable, respond_to?, send, singleton_class, singleton_method, singleton_methods, struct_class, tap, then, to_enum, to_json, to_native, to_param, to_query, to_s, try, try!, unescape, with_options, yield_self} <: #<Steep::Interface::Shape: type=::FFI::_DataConverter[::FFI::Pointer, instance, untyped], private?=false, methods={from_native, native_type, to_native}
│       { (::FFI::Pointer, untyped) -> ::FFI::ManagedStruct[::FFI::AutoPointer, untyped] } <: { (::FFI::Pointer, untyped) -> instance }
│         { (::FFI::Pointer, untyped) -> ::FFI::ManagedStruct[::FFI::AutoPointer, untyped] } <: (::FFI::Pointer, untyped) -> instance
│           (::FFI::Pointer, untyped) -> ::FFI::ManagedStruct[::FFI::AutoPointer, untyped] <: (::FFI::Pointer, untyped) -> instance
│             (::FFI::Pointer, untyped) -> ::FFI::ManagedStruct[::FFI::AutoPointer, untyped] <: (::FFI::Pointer, untyped) -> instance
│               ::FFI::ManagedStruct[::FFI::AutoPointer, untyped] <: instance
│                 ::FFI::Struct[untyped, untyped] <: ::FFI::ManagedStruct[::FFI::AutoPointer, untyped]
│                   ::Object <: ::FFI::ManagedStruct[::FFI::AutoPointer, untyped]
│
│ Diagnostic ID: RBS::UnsatisfiableTypeApplication
│
└     def self.auto_ptr: () -> Type::Mapped[
                               ~~~~~~~~~~~~~

../../.asdf/installs/ruby/3.3.4/lib/ruby/gems/3.3.0/gems/ffi-1.17.0-arm64-darwin/sig/ffi/struct.rbs:5:15: [error] Type application of `::FFI::Type::Mapped` doesn't satisfy the constraints: ::FFI::StructByReference[::FFI::Struct[::FFI::AbstractMemory, untyped], ::FFI::AbstractMemory] <: ::FFI::_DataConverter[::FFI::AbstractMemory, instance, untyped]
│   ::FFI::StructByReference[::FFI::Struct[::FFI::AbstractMemory, untyped], ::FFI::AbstractMemory] <: ::FFI::_DataConverter[::FFI::AbstractMemory, instance, untyped]
│     #<Steep::Interface::Shape: type=::FFI::StructByReference[::FFI::Struct[::FFI::AbstractMemory, untyped], ::FFI::AbstractMemory], private?=false, methods={!, !=, !~, <=>, ==, ===, Namespace, TypeName, __id__, __send__, acts_like?, as_json, blank?, class, class_eval, clone, deep_dup, define_singleton_method, display, dup, duplicable?, enum_for, eql?, equal?, extend, freeze, from_native, frozen?, hash, html_safe?, in?, inspect, instance_eval, instance_exec, instance_of?, instance_values, instance_variable_defined?, instance_variable_get, instance_variable_names, instance_variable_set, instance_variables, is_a?, itself, kind_of?, method, methods, native_type, nil?, object_id, presence, presence_in, present?, private_methods, protected_methods, public_method, public_methods, public_send, remove_instance_variable, respond_to?, send, singleton_class, singleton_method, singleton_methods, struct_class, tap, then, to_enum, to_json, to_native, to_param, to_query, to_s, try, try!, unescape, with_options, yield_self} <: #<Steep::Interface::Shape: type=::FFI::_DataConverter[::FFI::AbstractMemory, instance, untyped], private?=false, methods={from_native, native_type, to_native}
│       { () -> ::FFI::Type::Builtin } <: { (?(::FFI::ffi_auto_type | nil)) -> ::FFI::Type }
│         { () -> ::FFI::Type::Builtin } <: (?(::FFI::ffi_auto_type | nil)) -> ::FFI::Type
│           () -> ::FFI::Type::Builtin <: (?(::FFI::ffi_auto_type | nil)) -> ::FFI::Type
│             () -> ::FFI::Type::Builtin <: (?(::FFI::ffi_auto_type | nil)) -> ::FFI::Type
│               Incompatible arity: (?(::FFI::ffi_auto_type | nil)) and ()
│
│ Diagnostic ID: RBS::UnsatisfiableTypeApplication
│
└     type ptr = Type::Mapped[
                 ~~~~~~~~~~~~~

Detected 4 problems from 3 files
```
