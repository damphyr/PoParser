# Poparser

A Ruby PO file parser, editor and generator. PO files are translation files generated by GNU/Gettext tool. This GEM is compatible with [GNU PO file specification](https://www.gnu.org/software/gettext/manual/html_node/PO-Files.html). report misbehaviours and bugs, to the [issue tracker](https://github.com/arashm/PoParser/issues).

## Installation

Add this line to your application's Gemfile:

    gem 'poparser'

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install poparser

## Usage

Working with the GEM is pretty easy:

```ruby
path = Pathname.new('example.po')
po   = PoParser.parse(path)
=> <PoParser::Po, Translated: 68.1% Untranslated: 20.4% Fuzzy: 11.5%>
```

The `parse` method returns a `PO` object which contains all `Entries`:

```ruby
# get all entries
po.entries # or .all alias

# get all fuzzy entries
po.fuzzy

# get all untranslated entries
po.untranslated

# get all translated entries
po.translated

# returns a hash representation of the PO file
po.to_h

# returns a string representation of the PO file
po.to_s
```

You can add a new entry to the PO file:

```ruby
new_entry = {
              translator_comment: 'comment',
              refrence: 'refrence comment',
              msgid: 'untranslated',
              msgstr: 'translated string'
            }

po.add_entry(new_entry)

# There's also an alias for add_entry
po << new_entry
```

You can pass an array of hashes to `new_entry` and it will be added to `PO` file.

### Entry

Each entry can have following properties (for more information see [GNU PO file specification](https://www.gnu.org/software/gettext/manual/html_node/PO-Files.html)):

```
translator_comment
refrence
extracted_comment
flag
previous_untraslated_string
msgid
msgid_plural
msgstr
msgctxt
```

#### Working with entries

The `PO` object contains many `Entry` objects. Number of methods are available to check state of the `Entry`:

```ruby
entry.untranslated? # or .incomplete? alias
#=> false
entry.translated? # or .complete? alias
#=> true
entry.fuzzy?
#=> true
entry.plural?
#=> false
```

You can get or edit each of property of the `Entry`:

```ruby
entry.msgid
#=> "This is an msgid that needs to get translated"
entry.translate = "This entry is translated" # or msgstr= alias
entry.msgstr
#=> "This entry is translated"
```

You can mark an entry as fuzzy:

```ruby
entry.flag_as_fuzzy
entry.fuzzy?
#=> true
```

It's possible to get Hash and String representation of the `Entry`:

```ruby
entry.to_h
entry.to_s(true)
```

### Searching

`PO` is an `Enumerable`. All exciting methods from `Enumerable` are available in `PO`. the `PO` yields `Entry`.

### Saving
You can simply save the PO file using the `PO` object:

```ruby
po.save_file
```

If you want to save as the file in diffrent location change the `path`:

```ruby
po.path
#=> example.po
po.path = 'example2.po'
po.save_file
```

##To-Do

* Streaming support
* Better error reporting

## Contributing

1. Fork it ( http://github.com/arashm/poparser/fork )
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request
