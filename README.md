## Basic Autocomplete VIM Plugin for Kamailio Configuration ##

Implementation of a custom auto-complete function for Kamailio configuration
file.

See the next blog post for more details and screenshot:

  * http://www.kamailio.org/w/2014/09/kamailio-cfg-auto-complete-plugin-for-vim/

### Installation ###

Copy the kamailio.vim and kamailio-XYZ.dictionary files to:

```
  ~/.vim/plugin/kamailio/
```

From this folder, it can be done with following commands:

```
mkdir -p ~/.vim/plugin/kamailio
cp plugin/kamailio*.* ~/.vim/plugin/kamailio
```

### Activation ###

Once editing Kamailio configuration file, set the **completefunc** to
**KamailioComplete**:

```
:set completefunc=KamailioComplete
```

When using syntax higlighting and file type auto-detection VIM extensions for
Kamailio (shipped with kamailio sources, located in utils/misc/vim/), then you
can enable automatically the custome auto complete function by adding to
.vimrc:

```
  " custom complete function for kamailio config
  autocmd FileType kamailio setlocal completefunc=KamailioComplete
```

### Usage ###

While editing the Kamailio configuration file, use **CTRL+x CTRL+u** to
trigger auto-complete.

### Examples ###

(Note: parenthesis are used to surround control key sequences, they must
not be typed)

  * t_r(CTRL+x CTRL+u) -- pops up auto-complete box with all tokens starting
  with 't_r', such as t_relay(), t_reply(...), ...
  * $t(CTRL+x CTRL+u) -- pops up auto-complete box with all tokens starting
  with '$t', practically listing the config variables starting with 't', such
  as $tu, $time(name), ...

### Tips ###

Check next wiki page for other kinds of auto-completion that can be used
togheter with this plugin and can make writing of Kamailio configuration
file easier:

  * http://vim.wikia.com/wiki/Any_word_completion

Among them:

  * CTRL+n - complete with next matching word from current buffers
  * CTRL+p - complete with previous matching word from current buffers

### Dictionary ###

The plugin is searching for auto-complete matching in all files located in
plugin directory that have the name in the format *kamailio-XYZ.dictionary*.

XYZ can be anything, but the suggested format is *NM-DESC*:

  * NM - two digits to give the priority in the matching list
  * DESC - some suggestive id for the content

At this moment, the dictionary files are:

  * kamailio-20-modf.dictionary - containing functions from modules
  * kamailio-40-vars.dictionary - containing configuration variables
  * kamailio-50-core.dictionary - containing elements from core
  * kamailio-60-modp.dictionary - containing parameters from modules

Dictionaries for elements exported by modules are generated from docbook files
of respective modules. The others are more of custom scripting using various
sources or manual insertion.

Some of the dictinaries are far from complete (e.g., core).

The content of a dictionary file has the format:

```
replacement@@description
```

The *replacement* is what is matched against and is used to update current
word on autocomplete. All white spaces around it are removed.

The description apears as it is in the auto-complete box, with the purpose of
helper message.

The description contains the type of the replacement in between square brakets:

  * [func] - function
  * [param] - parameter
  * [var] - variable
  * [kword] - keyword

For example:

```
xlogl([ [facility,] level,] format)		@@-- [func] (xlog)
```

This dictionary record gives the prototype of xlog function from xlog module.

### Remarks ###

  * the records are in the order they are matched in dictionary files, thus
  it may require to scroll down in the auto-complete box to find desired
  token
  * it is not a global ordering, allowing to structure the view in
  auto-complete box based on dictionary file names and content
  * content in dictionary files is typically ordered, with default dictionary
  file, first are module functions, then config variables, module parameters
  and core elements
  * to get all the records ordered, merge all dictionary files in one (e.g.,
  kamailio-all.dictionary) and use **sort** command line tool to sort the
  records inside it. Then remove old dictionary files.
  * not doing sorting at runtim was done for performance reasons, although
  in the future can be added as config option for plugin

### To-Do ###

More like a whish list, no timelines committed for completing the items.

  * complete content for dictionaries
  * duplicate function records not to include the prototype of parameters (can
  save time when knowing what to type for parameters)
  * move the type of parameters to description (or add extra such record)

### Contributions ###

This plugin was developed mainly as an experiment, having basic prior knowledge
of VIM plugin development. Enhancements, including major refactoring, are more
than welcome.

Also, making the dictionary content more helpful for kamailio configuration
file writers is considered a major target. Feel free to give feedback and
suggestions.

### Copyright ###

(c) 2014-2016 Daniel-Constantin Mierla (asipto.com)

License: GPLv2
