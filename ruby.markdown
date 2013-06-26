# Ruby

Ruby coding standards seem to be more fluid and less hard and fast than in other languages, but that doesn't mean they're not there. The Ruby developer community thrives on convention.

## Casing rules

Ruby enforces a couple of simple casing rules: names beginning with uppercase letters refer to constants (including classes); anything else does not. So `NAME` and `Name` are constants, whereas `name`, `naMe`, and `_Name` are not. This is a property of the core language, as are the `@` and `$` prefixes for instance and global variables respectively.

Within these constraints, however, there is room for interpretation. The common practices among Rubyists appear to be as follows:

* `local_variable` (lowercase with underscores)
* `method_name` (ditto)
* `@instance_variable` (ditto, but with `@`)
* `ClassOrModuleName` (UpperCamelCase)
* `OTHER_CONSTANTS` (all caps with underscores)

Note in particular that Java-style lowerCamelCase is never used. The examples in <cite>Programming Ruby</cite> (the "Pickaxe Book") do use it, but this is to be considered an aberration, or perhaps a holdover from an earlier era of Ruby development. In 2010, the Ruby standard library and most other major libraries (including Rails) follow the casing rules I have outlined above, so it makes sense for new Ruby code to follow suit.

Also, note that the list above doesn't mention global variables. In properly written Ruby code, these are almost never necessary (with the exception of certain Perlisms such as `$_` and the match variables `$1`, `$2`...), but one legitimate use for them is to store global configuration values.  When they are used this way, I like to give them `$UNDERSCORED_CAPITAL` names, to emphasize their affinity with constants.

## Spacing

### Indentation

Use two-space indents, no tabs.

### Punctuation marks

**One space** on each side of **operators**, except `.` (if that's an operator!), which needs no space.

**One space** after **commas and semicolons**, none before.

**_No_ space** around **parentheses**, **square brackets**, or **braces** when used for Hashes. (For block braces, see [below](#blocks).)

## Code style

### Blocks

Ruby has two equivalent notations for blocks: `do`...`end` and `{`...`}`. It is pretty well established in the Ruby community that `do` is used for multiline blocks, while braces are used if the block fits on a single line:

```ruby
[1, 2, 3].each do |n|
  puts n
end
```
but
```ruby
[1, 2, 3].each {|n| puts n }
```

In practice, for a short block as in this example, the inline syntax is usually preferable. Multiline blocks should be used where there are multiple statements in the block, or where the single statement is long, complex, or otherwise easier to read on its own line.

For block braces, I like the following spacing (and I'm well aware that no two Ruby developers agree on this!):

**One space** before the opening brace.

**One space** after the block arguments, if present.

**One space** before the closing brace.

**One space** after the closing brace, unless the next character is a `.` for a method call.

Examples:

```ruby
3.times { puts 'Hello' }
```

```ruby
3.times {|i| puts i }
```

```ruby
[1, 2, 3].map {|n| n * 10 }.inspect
```

```ruby
[1, 2, 3].map do |n| # no braces here
  puts "Processing #{n}"
  n * 10
end
```

### Boolean operations

Ruby has two sets of Boolean operators: `&&`/`||` and `and`/`or`; the latter has lower precedence, as in Perl. Some style guides (like that of Rails) suggest that the latter only be used for flow control (e.g. `redirect_to :show and return`), and prefer the former set in all other cases. I disagree with this recommendation: to me, there is a lot of value in being able to write `if x > 5 and y < 3`, and the low precedence of `and` and `or` means I have fewer worries about parentheses (though in this case it doesn't actually make a difference). However, I wouldn't go much more complex than this: I think `if (x > 5 and y < 3) or z == 2` starts to be perhaps a little less readable than `if (x > 5 && y < 3) || z == 2` (note that while the parentheses are not strictly necessary in the last example, it's incomprehensible without them).

### Conditionals

`unless x` is equivalent to `if !x`, and `until x` is equivalent to `while !x`. Therefore, use whichever form is clearer. Generally, that means using `unless`/`until` for simple negative conditions, and `if`/`while` in all other cases, including complex negatives. That is, `unless color == :blue` is probably preferable to `if color != :blue`, but `unless color == :blue and vertices > 4` requires too much complex mental gymnastics to figure out which negation affects what, so `if color != :blue or vertices <= 4` is better in this case.

### Strings

Ruby has two primary quoting syntaxes for strings -- single and double quotes. Single-quoted strings do not interpolate variables or interpret backslash escape sequences other than `\'` and `\\`. Therefore, they are safer and probably (marginally) less processor-intensive.

Use double-quoted strings for the following cases:
* Strings containing interpolated variables
* Strings containing backslash sequences that single-quoted strings don't process
* Strings already containing single quotes

Prefer single-quoted strings elsewhere.

Note that if a string contains lots of single and double quotes and newlines, it is often clearer to use the generic quoting constructs (`%q` and `%Q`) or a heredoc.

String interpolation is usually more readable than `+` concatenation, and creates fewer `String` objects, so `full_name = "#{first_name} #{last_name}"` is generally preferable to `full_name = first_name + ' ' + last_name`. (Actually, in this case, `Array#join` is probably better than either of these.)