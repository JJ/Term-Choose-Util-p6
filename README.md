NAME
====

Term::Choose::Util - CLI related functions.

VERSION
=======

Version 0.001

DESCRIPTION
===========

This module provides some CLI related functions.

SUBROUTINES
===========

Values in brackets are default values.

choose_a_dir
------------

        $chosen_directory = choose_a_dir( { layout => 1, ... } )

With `choose_a_dir` the user can browse through the directory tree (as far as the granted rights permit it) and choose a directory which is returned.

To move around in the directory tree:

- select a directory and press `Return` to enter in the selected directory.

- choose the "up"-menu-entry ("`..`") to move upwards.

To return the current working-directory as the chosen directory choose "`.`".

The "back"-menu-entry ("`E<lt>`") causes `choose_a_dir` to return nothing.

As an argument it can be passed a hash. With this hash the user can set the different options:

  * clear_screen

If enabled, the screen is cleared before the output.

Values: 0,[1].

  * current

If set, `choose_a_dir` shows *current* as the current directory.

  * dir

Set the starting point directory. Defaults to the home directory or the current working directory if the home directory cannot be found.

  * enchanted

If set to 1, the default cursor position is on the "up" menu entry. If the directory name remains the same after an user input, the default cursor position changes to "back".

If set to 0, the default cursor position is on the "back" menu entry.

Values: 0,[1].

  * justify

Elements in columns are left justified if set to 0, right justified if set to 1 and centered if set to 2.

Values: [0],1,2.

  * layout

See the option *layout* in [Term::Choose](Term::Choose)

Values: 0,[1],2.

  * mouse

See the option *mouse* in [Term::Choose](Term::Choose)

Values: [0],1,2.

  * order

If set to 1, the items are ordered vertically else they are ordered horizontally.

This option has no meaning if *layout* is set to 2.

Values: 0,[1].

  * show_hidden

If enabled, hidden directories are added to the available directories.

Values: 0,[1].

choose_a_file
-------------

        $chosen_file = choose_a_file( { layout => 1, ... } )

Browse the directory tree like with `choose_a_dir`. Select "`E<gt>F`" to get the files of the current directory; than the chosen file is returned.

The options are passed with hash. See [#choose_a_dir](#choose_a_dir) for the different options. `choose_a_file` has no option *current*.

choose_dirs
-----------

        $chosen_directories = choose_dirs( { layout => 1, ... } )

`choose_dirs` is similar to `choose_a_dir` but it is possible to return multiple directories.

Different to `choose_a_dir`:

"`. `" adds the current directory to the list of chosen directories.

To return the chosen list of directories select the "confirm"-menu-entry "`= `".

The "back"-menu-entry ( "`E<lt> `" ) resets the list of chosen directories if any. If the list of chosen directories is empty, "`E<lt> `" causes `choose_dirs` to return nothing.

`choose_dirs` uses the same option as `choose_a_dir`. The option *current* expects as its value an array (directories shown as the current directories).

choose_a_number
---------------

        for ( 1 .. 5 ) {
            $current = $new
            $new = choose_a_number( 5, { current => $current, name => 'Testnumber' }  );
        }

This function lets you choose/compose a number (unsigned integer) which is returned.

The fist argument - "digits" - is an integer and determines the range of the available numbers. For example setting the first argument to 6 would offer a range from 0 to 999999.

The optional second argument is a hash with these keys (options):

  * clear_screen

If enabled, the screen is cleared before the output.

Values: 0,[1].

  * current

The current value. If set, two prompt lines are displayed - one for the current number and one for the new number.

  * name

Sets the name of the number seen in the prompt line.

Default: empty string ("");

  * mouse

See the option *mouse* in [Term::Choose](Term::Choose)

Values: [0],1,2.

  * thsd_sep

Sets the thousands separator.

Default: comma (,).

choose_a_subset
---------------

        $subset = choose_a_subset( @available_items, { current => @current_subset } )

`choose_a_subset` lets you choose a subset from a list.

As a first argument it is required an array which provides the available list.

The optional second argument is a hash. The following options are available:

  * clear_screen

If enabled, the screen is cleared before the output.

Values: 0,[1].

  * current

This option expects as its value the current subset of the available list (array). If set, two prompt lines are displayed - one for the current subset and one for the new subset. Even if the option *index* is true the passed current subset is made of values and not of indexes.

The subset is returned as an array.

  * index

If true, the index positions in the available list of the made choices is returned.

  * justify

Elements in columns are left justified if set to 0, right justified if set to 1 and centered if set to 2.

Values: [0],1,2.

  * layout

See the option *layout* in [Term::Choose](Term::Choose).

Values: 0,1,[2].

  * mouse

See the option *mouse* in [Term::Choose](Term::Choose)

Values: [0],1,2.

  * order

If set to 1, the items are ordered vertically else they are ordered horizontally.

This option has no meaning if *layout* is set to 2.

Values: 0,[1].

  * prefix

*prefix* expects as its value a string. This string is put in front of the elements of the available list before printing. The chosen elements are returned without this *prefix*.

The default value is "- " if the *layout* is 3 else the default is the empty string ("").

  * prompt

The prompt line before the choices.

Defaults to "Choose:".

change_config
-------------

    %tmp = change_config( @menu, %config, { in_place => 0 } )
    if %tmp {
        for keys %tmp.keys -> $key {
            %config{$key} = %tmp{$key};
        }
    }

The first argument is an array of arrays. These arrays have three elements:

  * the key/option name

  * the prompt string

  * an array with the available values of the option.

The second argument is a hash:

  * the keys are the option names

  * the values (`0` if not defined) are the indexes of the current value of the respective key.

        @menu = (
            [ 'enable_logging', "- Enable logging", [ 'NO', 'YES' ] ],
            [ 'case_sensitive', "- Case sensitive", [ 'NO', 'YES' ] ],
            ...
        );

        %config = (
            'enable_logging' => 0,
            'case_sensitive' => 1,
            ...
        );

The optional third argument can be used to pass a hash with the options.

When `change_config` is called, it displays for each array entry a row with the prompt string and the current value. It is possible to scroll through the rows. If a row is selected, the set and displayed value changes to the next. If the end of the list of the values is reached, it begins from the beginning of the list.

For the return-values see [#in_place](#in_place) below.

The option:

  * clear_screen

If enabled, the screen is cleared before the output.

Values: 0,[1].

  * in_place

If *in_place* is not enabled, a new configuration hash is created and returned. If nothing has changed `{}` is returned.

If *in_place* is enabled, the configuration hash (second argument) is edited in place. If the user has changed values change_config returns `{ changes =` $number_of_changes }> else `{}` is returned.

Values: 0,[1].

  * mouse

See the option *mouse* in [Term::Choose](Term::Choose)

Values: [0],1,2.

  * prompt

Overwrites the default prompt string.

SUPPORT
=======

You can find documentation for this module with the perldoc command.

    perldoc Term::TablePrint

AUTHOR
======

Matthäus Kiem <cuer2s@gmail.com>

CREDITS
=======

Thanks to the people from [Perl-Community.de](http://www.perl-community.de), from [stackoverflow](http://stackoverflow.com) and from [#perl6 on irc.freenode.net](irc://irc.freenode.net/#perl6) for the help.

LICENSE AND COPYRIGHT
=====================

Copyright 2016 Matthäus Kiem.

This library is free software; you can redistribute it and/or modify it under the Artistic License 2.0.
