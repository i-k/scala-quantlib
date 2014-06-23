scala-quantlib
==============

Using QuantLib with Scala

## Installing

### On Ubuntu

  1. Install [C++ Boost](http://boost.org) (>= 1.39) for building **QuantLib** and [SWIG](http://www.swig.org) for building its **Java Native Interfaces** (JNIs):
  
        sudo apt-get install libboost-all-dev swig
    
  2. Clone QuantLib from GitHub and build it (this can take a while):
  
        git clone https://github.com/lballabio/quantlib.git
        cd quantlib/QuantLib
        sh autogen.sh
        ./configure
        make # for multiple jobs at once: make --jobs 
        sudo make install
    
  3. Build the **QuantLib JNIs** using **SWIG**:
  
        cd ../QuantLib-SWIG
        sh autogen.sh
        #set JAVA_HOME if not already set, e.g. export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64
        ./configure --with-jdk-include=${JAVA_HOME}/include --with-jdk-system-include=${JAVA_HOME}/include/linux
        make -C Java
        sudo make install
    
  4. The resulting `QuantLib.jar` (and `.so`, etc.) are added to `/usr/local/lib`, so
  
  ###*either:*
  
    add it to `LD_LIBRARY_PATH` (when using e.g. **sbt**): 
  
        export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/lib
        
  ###*or:*
  
     run `java` with the option:
  
        -Djava.library.path=/usr/local/lib/


      On **Jenkins** the above code needs to be added to `JVM options`
  
  
  *Note that the JAR references the other files added to `/usr/local/lib`, so adding it alone isn't enough*
  
  5. If you're using **sbt**, add to `build.sbt`:
  
        unmanagedJars in Compile += file("/usr/local/lib/QuantLib.jar")
    
  6. Finally, in your *application's boot code* call:
  
        System.loadLibrary("QuantLibJNI")
    
