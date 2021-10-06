This repository contains a simple shell script for dealing with todo items directly from the command line. No fancy GUI or text-based interface, just simple invocations for getting that get the job done (and marking it as such!)

## Usage

After linking or copying or whatever you want the script somewhere that is accessible from your `$PATH`, you can start using the utility. There's no install script yet.

### Adding a new item
```
$ todo add 'Cook the spaghetti'
1
$ todo a 'Dance with the Devil'
2
```

As you can see, this invocation prints on standard output the ID of the item you just created. This is explained in more details in the next section. **Note that the script only reads the first letter of each command (`add`, `toggle`, etc.)**

### Marking an item as done
```
$ todo toggle 1
```

This command can also unmark an item (if for example it wasn't done after all), as its name implies. It takes an ID as a parameter, *not* the item's title as given in the previous section. This is because each todo item is given an ID when it is created, which is the output of the `add` command. This makes it easier to reference an item, and was a necessary thing to do because each invocation of this script is without memory of what happened in older invocations. More details on that in the next section about structure.

### Listing items
```
$ todo
1X       Cook the spaghetti
2.       Dance with the Devil
```

This is the default behavior of the script if you don't give it any argument, or if you give it garbage. As you can see, it outputs the title of the task, together with its ID and its state. The state is either a simple dot (which means the task is still 'to do'), or a cross 'X' (which means the task is done).

### Permanently removing an item
```
$ todo delete 2
Dance with the Devil
remove? [y/N] y
$ todo
2.       Dance with the Devil
```

Takes an item ID, confirm the deletion, and then does it. Notice that the ID of the remaining tasks are *not* changed. That's because a task ID does not reflect the order in which the tasks are displayed, nor does it reflect in some way the total number of tasks still in the databse. The ID is only there to make referencing a specific task easier.

### The nuclear command
```
$ todo nuke
nuke EVERYTHING by removing /PATH/TO/YOUR/TODO/DB? [y/N] y
$
```

This will completely delete all of your todo items and reset the ID counter by deleting the database. Be careful not to set `TODO_DB` to your filesystem's root (I'm joking... I think).

## Structure

I tried to do this as simple as possible. This program uses a 'database', which is simply a directory somewhere on your system. It defaults to `$HOME/.todo` if neither `TODO_DB` or `XDG_DATA_DIR` variables are set in the environment. A todo item is a subdirectory of the database that contains files associated with it, such as its title and whether or not it's marked as done. In order to easily reference items when issuing commands like `delete` or `toggle`, each item is given an ID when it's created. IDs are tracked using the 'id' file in the database directory. This ID is also the name of the todo item's directory.

A tree command is worth a thousand words. Here's a complete usage example:
```
$ rm -fr $YOUR_TODO_DB_PATH # start fresh for this example
$ todo a 'Do hot yoga'
1
$ todo a "Change time to match Moscow's" 1>/dev/null
$ echo 127 > $YOUR_TODO_DB_PATH/id
$ todo a 'Buy bus tickets to Burning Man'
128
$ todo t 1
$ todo
1X     Do hot yoga
128.   Buy bus tickets to Burning Man
2.     Change time to match Moscow's
$ tree $YOUR_TODO_DB_PATH
/YOUR/TODO/DB/PATH
├── 1
│   ├── done
│   └── title
├── 2
│   └── title
├── 128
│   └── title
└── id
```

The 'done' file you can see in this example is simply a marker, i.e., it contains nothing. Also, you can see that each item's directory is named after this item's ID.

All right, now go get stuff done.
