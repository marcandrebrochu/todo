This repository contains a simple shell script for dealing with todo items directly from the command line. No fancy GUI or text-based interface, just simple invocations for getting that get the job done (and marking it as such!)

## Usage

After linking or copying or whatever you want the script somewhere that is accessible from your `$PATH`, you can start using it as such.

### Adding a new item

```
$ todo add 'Cook the spaghetti'
1
$ todo a 'Dance with the Devil'
2
```

As you can see, this invocation prints on standard output the ID of the item you just created. This is explained in more details in the next section.

Note that the script only reads the first letter of each command (`add`, `toggle`, etc), so this is equivalent to the above example:

```
$ todo a 'Cook the spaghetti'
```

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

## Structure
