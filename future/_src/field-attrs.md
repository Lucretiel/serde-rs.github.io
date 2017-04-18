# Field attributes

- ##### `#[serde(rename = "name")]`

  Serialize and deserialize this field with the given name instead of its Rust
  name. This is useful for [serializing fields as camelCase](attr-rename.md) or
  serializing fields with names that are reserved Rust keywords.

- ##### `#[serde(default)]`

  If the value is not present when deserializing, use the `Default::default()`.

- ##### `#[serde(default = "path")]`

  If the value is not present when deserializing, call a function to get a
  default value. The given function must be callable as `fn() -> T`. For example
  `default = "empty_value"` would invoke `empty_value()` and `default =
  "SomeTrait::some_default"` would invoke `SomeTrait::some_default()`.

- ##### `#[serde(skip_serializing)]`

  Do not serialize this value.

- ##### `#[serde(skip_deserializing)]`

  Always use `Default::default()` or the function given by `default = "..."`
  instead of ever deserializing this value.

- ##### `#[serde(skip_serializing_if = "path")]`

  Call a function to determine whether to skip serializing this field. The given
  function must be callable as `fn(&T) -> bool`, although it may be generic over
  `T`. For example `skip_serializing_if = "Option::is_none"` would skip an
  Option that is None.

- ##### `#[serde(serialize_with = "path")]`

  Serialize this field using a function that is different from its
  implementation of `Serialize`. The given function must be callable as
  `fn<S>(&T, S) -> Result<S::Ok, S::Error> where S: Serializer`, although it
  may also be generic over `T`. Fields used with `serialize_with` are not
  required to implement `Serialize`.

- ##### `#[serde(deserialize_with = "path")]`

  Deserialize this field using a function that is different from its
  implementation of `Deserialize`. The given function must be callable as
  `fn<'de, D>(D) -> Result<T, D::Error> where D: Deserializer<'de>`, although it
  may also be generic over `T`. Fields used with `deserialize_with` are not
  required to implement `Deserialize`.

- ##### `#[serde(with = "module")]`

  Combination of `serialize_with` and `deserialize_with`. Serde will use
  `$module::serialize` as the `serialize_with` function and
  `$module::deserialize` as the `deserialize_with` function.

- ##### `#[serde(bound = "T: MyTrait")]`

  Where-clause for the `Serialize` and `Deserialize` impls. This replaces any
  trait bounds inferred by Serde for the current field.

- ##### `#[serde(bound(serialize = "T: MyTrait"))]`

  Where-clause for the `Serialize` impl.

- ##### `#[serde(bound(deserialize  = "T: MyTrait"))]`

  Where-clause for the `Deserialize` impl.

- ##### `#[serde(getter = "...")]`

  This is used when deriving `Serialize` for a [remote type](remote-derive.md)
  that has one or more private fields.