# tree-sitter-docker
A docker image with all the necessary libaries you need to build tree-sitter .so files


### Pre-text

We at [CodistAI](https://codist-ai.com/) use [tree-sitter](http://tree-sitter.github.io/tree-sitter/) a lot. 
Now, it depends on c-lang to be able to function properly. When one of our team-mate used Ubuntu, they found it
hard to compile some of the tree-sitter libaries (Following the instructions [here](https://github.com/tree-sitter/py-tree-sitter)) So, this little Dockerfile will let you install all the needed dependencies and then bring you right 
inside a iPython prompt where you can run the example code and build the libraries. 

### Building the docker image

Git clone this repo and then `cd` to it.

Then use the following command to build the image

```
docker build -t ubuntu-clang-image .
```

### Using the image

- Go to any directory. (For the sake of this example we assume that you created a directory tree like `docker_llvm/tree_sitter_libs` and you are the `docker_llvm` level)

- Run
```
docker run -it -v $PWD/tree_sitter_libs:/tree_sitter_libs --name ubuntu_clang ubuntu-clang-image
```

- You will be right in the middle of an iPython prompt.

- You will need to clone some of the tree-sitter libs. So let's say something like this 

```
>>> !git clone https://github.com/tree-sitter/tree-sitter-python
>>> !git clone https://github.com/tree-sitter/tree-sitter-php
```

- Once done, you should be able to use the following code to build the .so file
```
from tree_sitter import Language, Parser

Language.build_library(
  # Store the library in the `tree_sitter_libs` directory
  'tree_sitter_libs/my-languages.so',

  # Include one or more languages
  [
    'tree-sitter-php',
    'tree-sitter-python'
  ]
)
```

- Voila! you will have your .so in the `tree_sitter_libs` directory, ready to be used. 