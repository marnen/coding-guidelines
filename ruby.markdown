# Ruby

Ruby coding standards seem to be more fluid and less hard and fast than in other languages, but that doesn't mean they're not there. The Ruby developer community thrives on convention.

## Casing rules

Ruby enforces a couple of simple casing rules: names beginning with uppercase letters refer to constants (including classes); anything else does not. So `NAME` and `Name` are constants, whereas `name`, `naMe`, and `_Name` are not. This is a property of the core language, as are the `@` and `$` prefixes for instance and global variables respectively.

Within these constraints, however, there is room for interpretation. The common practices among Rubyists appear to be as follows:

* `local_variable` (lowercase with underscores)
* `method_name` (ditto)
* `@instance_variable` (ditto, but with `@`)
* `ClassName` (UpperCamelCase)
* `OTHER_CONSTANTS` (all caps with underscores)

Note in particular that Java-style lowerCamelCase is never used.

Also, note that the list above doesn't mention global variables. In properly written Ruby code, these are almost never necessary (with the exception of certain Perlisms such as `$_` and the match variables `$1`, `$2`...), but one legitimate use for them is to store global configuration values.  When they are used this way, I like to give them `$UNDERSCORED_CAPITAL` names, to emphasize their affinity with constants.

## Spacing

### Indentation

Use two-space indents, no tabs.