# bindebug versus objdebug Whats the difference

Both directories are used by Visual Studio when you build your solution.

The `obj` directory contains intermediate files generated from your source code. They are building blocks that are used in order to produce the final executable. This directory also acts as a cache between builds and allows you to do incremental compilation. Everything doesn't necessarily have to be recreated every time you do a build, which can speed up the build process.

The `bin` directory contains the final executable generated during the build. These are the files that you would typically give to a user.

Both directories contains subdirectories, one for each of your solution configurations as you build them. By default these are called `Debug` and `Release`, but you can create any number of configurations from within Visual Studio's configuration manager. These allow you to have different configurations depending on the kind of build that you want to do.

They are important in the sense that Visual Studio has to store the files from the build somewhere. But you can safely remove them if you don't want them. They will be recreated when needed.
