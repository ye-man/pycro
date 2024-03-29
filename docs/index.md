
> this README file is generated by `$ ./pycro -L markdown README.pycro.md` command.

# pycro

```
$ wc pycro
 2593  5890 72636 pycro
```

contents:
- [introduction](#introduction)
- [usage example](#usage-example)
- [installation](#installation)
- [documentation](#documentation)
- [contributing](#contributing)

## introduction
Pycro is a python integrated macro preprocessor. It will interpret input texts
and generates a corresponding python code that will generate the intended
result, if we compile and execute that.

## usage example

imagine we have this `main.c` file:

```c

#include <stdio.h>

//# names = ['Oliver', 'Jack', 'Harry', 'James', 'John']

int main()
{
	//@ for name in names:
	printf("Hello ${name}!\n");
	//@ end for
	return 0;
}

```

we open this file and pass it to generate\_code function:

```python
#!/usr/bin/python3

import pycro

env = pycro.CompilerEnvironment(language = 'c')

with open('main.c') as infile:
    with open('main.c.py', 'w') as outfile:
        pycro.generate_code(infile, outfile, env)

```

another file will be created named `main.c.py`:

```python
__outfile__.write('\n');
__outfile__.write('#include <stdio.h>\n');
__outfile__.write('\n');
names = ['Oliver', 'Jack', 'Harry', 'James', 'John']
__outfile__.write('\n');
__outfile__.write('int main()\n');
__outfile__.write('{\n');
for name in names:
	__outfile__.write('\tprintf("Hello ');__outfile__.write(str(name));__outfile__.write('!\\n");\n');
__outfile__.write('\treturn 0;\n');
__outfile__.write('}\n');
__outfile__.write('\n');
```

now we can compile and execute `main.c.py` using the following code:

```python
#!/usr/bin/python3

import pycro

env = pycro.ExecutorEnvironment()

with open('main.c.py') as infile:
    with open('_main.c', 'w') as outfile:

        code_object = pycro.compile_generated_code(infile.read(), infile.name)

        pycro.execute_code_object(code_object, outfile, env)

```

the generated result saved as `_main.c`:

```c

#include <stdio.h>


int main()
{
	printf("Hello Oliver!\n");
	printf("Hello Jack!\n");
	printf("Hello Harry!\n");
	printf("Hello James!\n");
	printf("Hello John!\n");
	return 0;
}

```

it's time to compile `_main.c`:

```
$ gcc _main.c -o main
```

and run `./main`:

`$ ./main`:

```
Hello Oliver!
Hello Jack!
Hello Harry!
Hello James!
Hello John!
```


## installation
> coming soon ...

## documentation
- [How pycro works](#How-pycro-works)
- [API](#API)
- [command line interface](#command-line-interface)

### How pycro works
> coming soon

### API
> coming soon ...

### command line interface
__`$ ./pycro --help`__:
```
usage: ./pycro [OPTION]... [[--] FILE | -]...
Pycro FILEs. if no FILE or if FILE is '-', standard input is read. write to
standard output if no output specified.

Operation modes:
    -h, --help                      display this help and exit
    --version                       display pycro version and exit
    -a, --arrange-process           perform Sortable OPTIONs and FILEs
                                      according to their orders

Sortable options:
    -D, --define NAME[=VAR]         define NAME variable as having VALUE, or
                                      None
    -U, --undefine NAME             undefine NAME variable
    -S, --set KEY=VALUE             set KEY setting to VALUE
    -L, --lang LANGUAGE             set prefixes and suffixes for LANGUAGE
                                      specification
    -l, --load JSONFILE             load JSONFILE and update variables
    -I, --import MODULE             import MODULE to interpreter environment
    -- FILE                         read input FILE (don't treats '-' as
                                      standard input)

Common options:
    -n, --filter-name PATTERN       filter input FILEs by its name match
                                      shell PATTERN
    -p, --filter-path PATTERN       filter input FILEs by its path match
                                      shell PATTERN
    -N, --ignore-name PATTERN       ignore any input FILEs that its name match
                                      shell PATTERN
    -P, --ignore-path PATTERN       ignore any input FILEs that its path match
                                      shell PATTERN
    -f, --force                     overwrite existing files
    -r, --recursive                 pycro directories recursively
    -C, --clear-cache               first clear compiler cache
    -d, --dereference               follow symbolic links
    -o, --outfile OUTFILE           set output file to OUTFILE
    -O, --outfolder OUTFOLDER       set output folder to OUTFOLDER

Known language specifications:
    c, cpp, html, java, javascript, markdown, perl, python

Setting keys:
    mp, macro_prefix                macro line prefix
    ms, macro_suffix                macro line suffix

    sp, statement_prefix            statement line prefix
    ss, statement_suffix            statement line suffix

    cp, comment_prefix              comment line prefix
    cs, comment_suffix              comment line suffix

    vp, variable_prefix             variable substitution prefix
    vs, variable_suffix             variable substitution suffix

    ep, evaluation_prefix           evaluation substitution prefix
    es, evaluation_suffix           evaluation substitution suffix
```

## contributing

__notes:__

- first take a look at [makefile](makefile).

__todos:__

in `pycro`:
```
230: # TODO: remove debugging codes on final release
1394: # TODO: complete _generator_include()
1425: # TODO: write _generate_load()
1453: # TODO: add 'include' when ready
1457: # TODO: add 'load' when ready
1901: # TODO: write _include_function
1951: # TODO: write _load_function
2011: # TODO: complete __apply_config_filters
2355: # TODO: walk into directories
2381: # TODO: complete arranged performace
2410: # TODO: complete multiprocessing
2453: # TODO: write '--outfile', '--outfolder' functionality
2500: # TODO: complete _OUTFOLDER_FLAG
```

## license
__WTFPL__:
```
            DO WHAT THE FUCK YOU WANT TO PUBLIC LICENSE
                    Version 2, December 2004

 Copyright (C) 2019 Mohammad Amin Khakzadan <mak12776@gmail.com>

 Everyone is permitted to copy and distribute verbatim or modified
 copies of this license document, and changing it is allowed as long
 as the name is changed.

            DO WHAT THE FUCK YOU WANT TO PUBLIC LICENSE
   TERMS AND CONDITIONS FOR COPYING, DISTRIBUTION AND MODIFICATION

  0. You just DO WHAT THE FUCK YOU WANT TO.

```


