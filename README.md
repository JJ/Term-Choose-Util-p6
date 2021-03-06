[![Build Status](https://travis-ci.org/kuerbis/Term-Choose-Util-p6.svg?branch=master)](https://travis-ci.org/kuerbis/Term-Choose-Util-p6)

NAME
====

Term::Choose::Util - TUI-related functions for selecting directories, files, numbers and subsets of lists.

DESCRIPTION
===========

This module provides TUI-related functions for selecting directories, files, numbers and subsets of lists.

CONSTRUCTOR
===========

The constructor method `new` can be called with optional named arguments:

        my $new = Term::Choose::Util.new( :mouse(1), ... )

ROUTINES
========

Values in brackets are default values.

### Options available for all subroutines

  * clear-screen

If enabled, the screen is cleared before the output.

Values: [0],1.

  * color

Enables the support for color and text formatting escape sequences.

Setting color to 1 enables the support for color and text formatting escape sequences except for the current selected element. If set to 2, also for the current selected element the color support is enabled (inverted colors).

Values: [0],1,2.

  * hide-cursor

Hide the cursor

Values: 0,[1].

  * info

A string placed on top of of the output.

Default: undef

  * mouse

Enable the mouse mode. An item can be chosen with the left mouse key, the right mouse key can be used instead of the SpaceBar key.

Values: [0],1.

  * cs-label

The value of *cs-label* is a string which is placed in front of the "chosen so far" info output.

With `settings-menu` the "chosen so far" info output is only shown if *cs-label* is defined.

Defaults: `choose-directories`: '> ', `choose-a-directory`: 'Dir: ', `choose-a-file`: 'File: ', `choose-a-number`: 'Dirs: ', `choose-a-subset`: '', `settings-menu`: undef

The "chosen so far" info output is placed between the *info* string and the *prompt* string.

  * prompt

A string placed on top of the available choices.

Default: undef

  * back

Customize the string of the menu entry "back".

Default: `BACK`

  * confirm

Customize the string of the menu entry "confirm".

Default: `CONFIRM`.

choose-a-directory
------------------

        $chosen-directory = choose-a-directory( :layout(1), ... )

With `choose-a-directory` the user can browse through the directory tree and choose a directory which is returned.

To move around in the directory tree:

- select a directory and press `Return` to enter in the selected directory.

- choose the "parent-dir" menu entry to move upwards.

To return the current working-directory as the chosen directory choose the "confirm" menu entry.

The "back" menu entry causes `choose-a-directory` to return nothing.

Following options can be set:

  * alignment

Elements in columns are aligned to the left if set to 0, aligned to the right if set to 1 and centered if set to 2.

Values: [0],1,2.

  * init-dir

Set the starting point directory. Defaults to the home directory (`$*HOME`).

  * enchanted

If set to 1, the default cursor position is on the "parent-dir" menu entry. If the directory name remains the same after an user input, the default cursor position changes to "back".

If set to 0, the default cursor position is on the "back" menu entry.

Values: 0,[1].

  * layout

See the option *layout* in [Term::Choose](https://github.com/kuerbis/Term-Choose-p6)

Values: 0,[1],2.

  * order

If set to 1, the items are ordered vertically else they are ordered horizontally.

This option has no meaning if *layout* is set to 2.

Values: 0,[1].

  * show-hidden

If enabled, hidden directories are added to the available directories.

Values: 0,[1].

  * parent-dir

Customize the string of the menu entry "parent-dir".

Default: PARENT-DIR

  * [Options available for all subroutines](#Options available for all subroutines)

choose-a-file
-------------

        $chosen-file = choose-a-file( :layout(1), ... )

Browse the directory tree the same way as described for `choose-a-directory`. Select the "show-files" menu entry to get the files of the current directory. To return the chosen file select the "confirm" menu entry.

Options as in [choose-a-directory](#choose-a-directory) plus

  * filter

If set, the value of this option is treated as a regex pattern.

Only files matching this pattern will be displayed.

The regex pattern is used as the value of `dir`s `:test` parameter.

  * show-files

Customize the string of the menu entry "show-files".

Default: `[Show-Files]`

choose-directories
------------------

        @chosen-directories = choose-directories( :layout(1), ... )

`choose-directories` is similar to `choose-a-directory` but it is possible to return multiple directories.

Use the "add-dirs" menu entry to add the current directory to the list of chosen directories.

To return the list of chosen directories select the "confirm" menu entry.

The "back" menu entry removes the last added directory. If the list of chosen directories is empty, "back" causes choose-directories to return nothing.

Options as in [choose-a-directory](#choose-a-directory) plus

  * add-dirs

Customize the string of the menu entry "add-dirs".

Default: `[Add-Dir]`

choose-a-number
---------------

        my $number = choose-a-number( 5, :cs-label<Testnumber>, ... );

This function lets you choose/compose a number (unsigned integer) which is then returned.

The fist argument is an integer and determines the range of the available numbers. For example setting the first argument to `4` would offer a range from `0` to `9999`. If not set, it defaults to `7`.

Options:

  * small-first

Put the small number ranges on top.

  * thousands-separator

Sets the thousands separator.

Default: `,`

  * [Options available for all subroutines](#Options available for all subroutines)

choose-a-subset
---------------

        $subset = choose-a-subset( @available-items, :layout( 1 ), ... )

`choose-a-subset` lets you choose a subset from a list.

The subset is returned as an array.

The first argument is the list of choices.

Options:

  * all-by-default

If enabled, all elements are selected if `CONFIRM` is chosen without any selected elements.

  * alignment

Elements in columns are aligned to the left if set to 0, aligned to the right if set to 1 and centered if set to 2.

Values: [0],1,2.

  * index

If true, the index positions in the available list of the made choices are returned.

Values: [0],1.

  * keep-chosen

If enabled, the chosen items are not removed from the available choices.

Values: [0],1;

  * layout

See the option *layout* in [Term::Choose](Term::Choose).

Values: 0,1,2,[3].

  * mark

Expects as its value a reference to an array with indexes. Elements corresponding to these indexes are pre-selected when `choose-a-subset` is called.

  * order

If set to 1, the items are ordered vertically else they are ordered horizontally.

This option has no meaning if *layout* is set to 3.

Values: 0,[1].

  * prefix

*prefix* expects as its value a string. This string is put in front of the elements of the available list in the menu. The chosen elements are returned without this *prefix*.

Default: empty string.

  * cs-begin

Info output: the *cs-begin* string is placed between the *cs-label* string and the chosen elements as soon as an element has been chosen.

Default: empty string

  * cs-separator

Info output: *cs-separator* is placed between the chosen list elements.

Default: `,`

  * cs-end

Info output: as soon as elements have been chosen the *cs-end* string is placed at the end of the chosen elements.

Default: empty string

  * [Options available for all subroutines](#Options available for all subroutines)

To return the chosen subset select the "confirm" menu entry.

The "back" menu entry removes the last added chosen items. If the list of chosen items is empty, "back" causes `choose-a-subset` to return nothing.

settings-menu
-------------

        my @menu = (
            ( 'enable-logging', "- Enable logging", ( 'NO', 'YES' )   ),
            ( 'case-sensitive', "- Case sensitive", ( 'NO', 'YES' )   ),
            ( 'attempts',       "- Attempts"      , ( '1', '2', '3' ) )
        );

        my %config = (
            'enable-logging' => 0,
            'case-sensitive' => 1,
            'attempts'       => 2
        );

        settings-menu( @menu, %config, :1mouse, ... );

The first argument is a list of lists. Each of the lists have three elements:

    the option name

    the prompt string

    a list of the available values for the option

The second argument is a hash:

    the hash key is the option name

    the hash value (zero based index) sets the current value for the option.

This hash is edited in place: the changes made by the user are saved in this hash.

Options: see [Options available for all subroutines](#Options available for all subroutines).

When `settings-menu` is called, it displays for each list entry a row with the prompt string and the current value.

It is possible to scroll through the rows. If a row is selected, the set and displayed value changes to the next.After scrolling through the list once the cursor jumps back to the top row.

If the "back" menu entry is chosen, `settings-menu` does not apply the made changes and returns nothing. If the "confirm" menu entry is chosen, `settings-menu` applies the made changes in place to the passed configuration hash (second argument) and returns the number of made changes.

Setting the option *cs-label* to a defined value adds an info output line.

DEPRECATIONS
------------

The use of `choose-dirs` is deprecated - use `choose-directories` instead.

The use of `choose-a-dir` is deprecated - use `choose-a-directory` instead.

The deprecated routine names will be removed.

RENAMED OPTIONS
---------------

    <Old names>:                <New names>:

    justify                     alignment

    dir                         init-dir

    up                          parent-dir

    name                        cs-label

    current-selection-label     cs-label

    sofar-begin                 cs-begin

    current-selection-begin     cs-begin

    sofar-separator             cs-separator

    current-selection-separator cs-separator

    sofar-end                   cs-end

    current-selection-end       cs-end

    thsd-sep                    thousands-separator

    add-dir                     add-dirs

Only the new option names work.

AUTHOR
======

Matthäus Kiem <cuer2s@gmail.com>

CREDITS
=======

Thanks to the people from [Perl-Community.de](http://www.perl-community.de), from [stackoverflow](http://stackoverflow.com) and from [#perl6 on irc.freenode.net](irc://irc.freenode.net/#perl6) for the help.

LICENSE AND COPYRIGHT
=====================

Copyright 2016-2020 Matthäus Kiem.

This library is free software; you can redistribute it and/or modify it under the Artistic License 2.0.

