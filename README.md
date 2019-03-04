# ROPTLIB
ROPTLIB: Riemannian Manifold Optimization Library


The library is used to solve an optimization problem

f(x), x \in \mathcal{M},

where \mathcal{M} is a Riemannian manifold.

The detalied instruction and user manual can be found at http://www.math.fsu.edu/~whuang2/Indices/index_ROPTLIB.html.

### Compilation in C++

- Use Makefile -> all tests in a single file (DriverCpp)
  ```shell
  cd ${path/to/ROPTLIB}
  make ROPTLIB -j8
  ./DriverCpp
  ```
- Use cmake
  ```shell
  cd ${path/to/ROPTLIB}
  mkdir build
  cd build
  cmake ..
  make -j8
  sudo make install
  ```

### Generate the documentation files
The generated files will be stored in `./doc`
  ```shell
  doxygen doxyfile.cfg
  ```
