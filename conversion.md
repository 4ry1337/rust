# Conversion
Primitive types can be converted to each other through [[3. types#Casting|casting]].

Rust addresses conversion between custom types by the use of [[traits]].

The generic conversions will use the `From` and `Into` traits.

However there are more specific ones for the more common cases, in particular when converting to and from Strings.
