# PHP Style Guide

# Format

## File

All PHP files MUST use the Unix LF (linefeed) line ending only.

All PHP files MUST end with a non-blank line, terminated with a single LF.

## Indent

Code MUST use an indent of 4 spaces for each indent level, and MUST NOT use tabs for indenting.

## Line

There MUST NOT be trailing whitespace at the end of lines.

There MUST NOT be more than one statement per line.

Each method in a Query's chain call is **RECOMMENDED** start on a new line.

```php
Foo::query()
    ->where(Foo::COLUMN_PARENT_ID, 1)
    ->where(Foo::COLUMN_TYPE, FooType::BAZ)
    ->where(function (Builder $query): void {
        $query->whereNotIn(Foo::COLUMN_SUBTYPE, ['foo','bar','baz'])
            ->orWhereNull(Foo::COLUMN_SUBTYPE);
    })
    ->pluck(Foo::COLUMN_NAME, Foo::COLUMN_ID)
    ->all();
```

## Function calls

```php
$res = calculate($model, $param1, $param2, $param3);
$res = http_build_query($params, encoding_type: PHP_QUERY_RFC3986);
```

Vertical parameter list style **MAY** be used when there are too many parameters. Each argument **SHOULD** be placed in a separated line. All arguments **MUST** be aligned by one indent from the beginning of function name. The last argument **MUST** be followed by a comma.

```php
$res = calculate(
    $model,
    $param1,
    $param2,
    $param3,
);
```

When calling function with named arguments in multiple lines, all arguments **MUST** be aligned.

```php
setcookie(
    name              : 'x-token',
    value             : $this->generateToken(),
    expires_or_options: Date::now()->addSeconds($this->ttl)->timestamp(),
    path              : '/',
    secure            : true,
    httponly          : true,
);
```

## Explicit type declaration

All functions are **REQUIRED** to have both **argument** type and **return** type declared.

```php
// RECOMMENDED using union types to replace nullable types
~~function foo(array $bar1, ?string $bar2): ?array
{
    return $bar2 === 'magic' ? $bar1 : null;
}~~
// Nullable types to union types
function foo(array $bar1, string|null $bar2): array|null
{
    return $bar2 === 'magic' ? $bar1 : null;
}

// Void return type
function set(int $value): void
{
    $this->var = $value;
}

// Union types
function get_user(int $id): User|null
{
    // ...
}

// Intersection types
function create_reader(UnitEnum&FileTypeInterface $type): Reader
{
    // ...
}

// Resource
function read_file_lines($file_handle): Generator
{
    while(!feof($file_handle)) {
        $line = fgets($file_handle);

        if ($line !== false) {
            yield $line;
        }
    }
}
```

# Naming

<aside>
💡 When using pascal and camel cases, acronyms, such as API and SSO, **MUST** be transformed to the corresponding letter case too. For example, "LPL SSO API" would be written as `LplSsoApi` (studly case) or `lplSsoApi`  (camel case), as opposed to `LPLSSOAPI`.

</aside>

## Namespace, class, interface, trait and enum names

The name of namespace, class, interface, trait and enum **MUST** be in StudlyCaps.

Trait name is **RECOMMENDED** to be choose one of the follow

1. Third-person singular verb
    
    ```php
    trait VerifiesEmail
    {
    }
    ```
    
2. Adjective
    
    ```php
    trait EmailVerifiable
    {
    }
    ```
    

## Class method names

Class method, including static method, names **MUST** be in camelCase.

```php
class QueryParser
{
    public function sampleMethod(): void
    {
    }

    public static function fromString(string $query): static
    {
    }
}
```

However, the names of class instances (objects), properties and static properties should follow the rules outlined in [Variable names](https://www.notion.so/fd48fc1ffbc8434580ac9d0f66bd39f4?pvs=21) section below.

```php
$query_parser = new QueryParser();
$query_parser->encoding_type = QueryParser::RFC3986;
```

## Function names

Function (as opposed to class method) names **MUST** be in snake case, e.g.

```php
function validate_input(): void
{
}
```

## Variable names

Variable names, including class property and static property names **MUST** be in snake_case e.g. `$new_password`.

Names for array variables **SHOULD** be plural, such as `$colors` and `$accounts`.

```php
$colors = ['green', 'yellow'];
$color  = 'blue';
```

Names for associative arrays **MUST** use `$values_by_key` format, e.g.

```php
$colors_by_person_name = [
    'alice' => 'green',
    'bob'   => 'yellow',
];
```

Two-dimensions associative arrays **MUST** use `$value_lists_by_key` format, e.g.

```php
$color_lists_by_theme = [
	'dark'  => ['black', 'grey'],
	'light' => ['white', 'green'],
];
```

`foreach` **SHOULD** use plural to singular-form of a noun.

```php
foreach ($colors as $id => $color) {
}
```

## Variable naming conventions

```php
$login_user // currently logged-in user
$user // context user
$household // context household
$fqcn // fully-qualified class name, with or without leading backslash, e.g. '\App\Models\Account'
$class_name // unqualified class name, e.g. 'Account'
$table_name // e.g. 'accounts'
$model // class name, in most cases $fqcn is prefered
$object // an instance of a class, e.g. $object = new $fqcn();
```

## Constant names

Constant names **MUST** be in uppercase with underscore as separator, e.g. `ANONYMOUS_USER`.

## Enum cases

Enum cases **MUST** be in uppercase with underscope as separator.

```php
enum AccountType: string
{
    case BANK       = 'bank';
    case CARD       = 'card';
    case INVESTMENT = 'investment';
    case LOAN       = 'loan';
    case STOCK_PLAN = 'stock_plan';
}
```

# Comment

## Indent and capitalization

The comment **MUST** have the same indent as the its following statements or control structure.

```php
// Check if authorized
if (Auth::check()) {
    // Do the work
    $this->do();
}
```

## Capitalization and space

All comment, single line or block, **MUST** have the first word capitalized, unless the comment is placed at the end of a statement.

A space should be placed between `//` and the comment content.

```php
// Capitalized comment
$fruit = 'apple'; // no need to capitalize
```

## Block comments

If a block comment contains any PHPDoc notations, **MUST** use docblock style

```php
/**
 * This is a comment, @link [https://www.google.com/](https://www.google.com/)
 */
```