# maali
Pawsey Supercomputing Centre Build System

You should be able to install maali by using:

* `git clone https://github.com/Pawseyops/maali`
* `cd maali`
* `MAALI_DEFAULT_COMPILERS='gcc/4.8.3 intel/15.0.0' ./maali -t maali -v 1.0b1`

where `gcc/4.8.3` and `intel/15.0.0` are modulefiles for GCC 4.8.3 and the Intel Compiler 15.0.0 respectively.

For Cray Systems we use something like:

```
MAALI_DEFAULT_CRAY_PRGENVS='PrgEnv-cray/5.2.25 PrgEnv-gnu/5.2.25 PrgEnv-intel/5.2.25' \
MAALI_DEFAULT_CRAY_PES='sandybridge ivybridge haswell' \
MAALI_DEFAULT_COMPILERS='gcc/4.8.2 gcc/4.9.2 intel/14.0.1.106 cce/8.3.0 cce/8.3.12' \
MAALI_DEFAULT_PYTHON='python/2.6.9 python/2.7.10' \
./maali -t maali -v 1.0b1
```



These variable match up to this way in cygnet files.
```
# specify which PrgEnv we want to build the tool with
MAALI_TOOL_CRAY_PRGENV="$MAALI_DEFAULT_CRAY_GCC_PRGENV $MAALI_DEFAULT_CRAY_INTEL_PRGENV"

# specify which cpus to target
MAALI_TOOL_CRAY_CPU_TARGET="$MAALI_DEFAULT_CRAY_PES"

# specify which compilers we want to build the tool with
MAALI_TOOL_COMPILERS="$MAALI_DEFAULT_COMPILERS"
```

## list of maali functions

function maali_wiki {         line 9-18
function maali_build {        line 83-93
function maali_cmake_build {  line 97-114
function maali_download {     line 118-174
function maali_genpkgconfig { line 178-197
function maali_git_download { line 201-237
function maali_load_module {  line 241-269
function maali_makedir {      line 273-315
function maali_module {       line 319-803
function maali_python_build { line 807-822
function maali_run {          line 826-847
function maali_r_build {      line 851-856 
function maali_unpack {       line 860-918
function log {                line 923-927
function removedir {          line 931-937
function removefile {         line 941-945


##  Definitions
Definitions of the ~250 variables used in maail.

*MAALI_APPS_DIR* -  Directory where the software is installed is based 
 on the tool type
<code>MAALI_APPS_DIR="$MAALI_ROOT/$MAALI_TOOL_TYPE"</code></pre>

*MAALI_APP_HOME* - Used in function maali_module to define application 
  base directory path using the compiler/compiler version hierarchy
  for include, bin, manpath etc..
<pre><code>MAALI_APP_HOME="$MAALI_APPS_DIR/\$env(COMPILER)/\$env(COMPILER_VER)/$MAALI_TOOL_NAME/$MAALI_TOOL_VERSION" </code></pre>

*MAALI_APP_HOME_NAME* - Used in the maali_module function to create a environment variable for the module 
<pre><code>MAALI_APP_HOME_NAME="MAALI_"$MAALI_TOOL_NAME_UPPERCASE"_HOME"</code></pre>

*MAALI_LOADED_PRGENV* - Used in the maali_module function when setting the variable MAALI_APP_HOME path variable and set as an evironment module variable for use with Cray systems.
<pre><code>setenv MAALI_LOADED_PRGENV PrgEnv-cray</code></pre>

*MAALI_TOOL_NAME* - input variable for the maali command, name of the tool/application being installed ( -t flag )

*MAALI_TOOL_VERSION* - input variable for the maali command, version of the tool/application being installed ( -v flag )

*MAALI_BUILDER_BUILD_CN* - for system build define ldap details cn
<pre><code>MAALI_BUILDER_BUILD_CN=`ldapsearch -LLL -x '(uid='$MAALI_BUILDER_UID')' cn | grep cn: | sed -e 's/cn: //g'` </code></pre>

*MAALI_BUILDER_BUILD_MAIL* - for system build define ldap details email
<pre><code>MAALI_BUILDER_BUILD_MAIL=`ldapsearch -LLL -x '(uid='$MAALI_BUILDER_UID')' mail | grep mail: | sed -e 's/mail: //g'` </code></pre>

*MAALI_BUILDER_UID* - input variable for the maali command, for system builds, envivironment variable that maali checks.

*MAALI_BUILDONLY* - input variable for the maali command, skips the module file creation.

*MAALI_BUILD_DATE* - timestamp of when system build was done.

*MAALI_BUILD_DIR* - default build directory
<pre><code>MAALI_BUILD_DIR="$MAALI_ROOT/build" </code></pre>

*MAALI_BUILD_PREREQ_MODULE* - loop index name 
<pre><code> for MAALI_BUILD_PREREQ_MODULE in $MAALI_TOOL_BUILD_PREREQ; do</code></pre>

*MAALI_BUILD_PREREQ_MODULE_NAME* - name of module used with version cut off
<pre><code>MAALI_BUILD_PREREQ_MODULE_NAME=`echo $MAALI_BUILD_PREREQ_MODULE | cut -d '/' -f 1` </code></pre>

*MAALI_CMAKE_PATH* - defines path where cmake command will be run.
<pre><code>MAALI_CMAKE_PATH=".." </code></pre>

*MAALI_CMAKE_TOOL* - boolean environment variable indicating that the build will use cmake instead of make.

*MAALI_CMDLINE* - captures the maali command line input arguments
<pre><code>MAALI_CMDLINE=$* </code></pre>

*MAALI_COMPILER* - used as an index counter in a for loop listing.
<pre><code> for MAALI_COMPILER in $MAALI_TOOL_COMPILERS; do </code></pre>

*MAALI_TOOL_COMPILERS defined as the list of compiler to build against and part of the installation path.

*MAALI_COMPILER_FOUND_MATCH* - used in the maali_module function boolean value

*MAALI_COMPILER_NAME* - module name of compiler with the version truncated.

*MAALI_COMPILER_NUMBER* - is the number of compilers listed in the MAALI_TOOL_COMPILERS variables.  ( looks unused? )
<pre><code>MAALI_COMPILER_NUMBER=`echo "$MAALI_TOOL_COMPILERS" | wc -w` </code></pre>

*MAALI_COMPILER_TYPE* - used as a loop variable in maali_module function part of the MAALI_SUPPORTED_COMPILERS list
<pre><code>for MAALI_COMPILER_TYPE in $MAALI_SUPPORTED_COMPILERS; do </code></pre>

*MAALI_COMPILER_VERSION* - is the compiler version number it is obtained different way depending on they compiler.
<pre><code>MAALI_COMPILER_VERSION=`cc -V | head -1` </code></pre>
 
*MAALI_COMPILER* - is the variable used in loops
<pre><code>for MAALI_COMPILER in $MAALI_TOOL_COMPILERS; do</code></pre>

*MAALI_CONFIG* - is the variable used to define the maali config file name.
<pre><code>MAALI_CONFIG=$OPTARG</code></pre>

*MAALI_CORES* - for use in parallel make -j $MAALI_CORES

*MAALI_CRAY* - boolean variable to set if this is a cray system.

*MAALI_CRAY_COMPILER_MATCH* - string variable to set to the different cray compiler types - gcc, intel, cce

*MAALI_CRAY_CPU_TARGET* - string variable defines cpu target used
 <pre><code>  maali_load_module "craype-$MAALI_CRAY_CPU_TARGET"</code></pre>
(NEEDS to be standardize!!)

*MAALI_CRAY_CPU_TARGET_FOUND_MATCH - boolean variable in maali_module function

*MAALI_CRAY_CPU_TARGET_TYPE* - is the loop variable in the maali_module function
<pre><code>for MAALI_CRAY_CPU_TARGET_TYPE in $MAALI_SUPPORTED_CRAY_CPU_TARGETS; do</code></pre>


*MAALI_CRAY_MODULE* - is a loop variable used in the maali main script 
<pre><code>for MAALI_CRAY_MODULE in $MAALI_EXTRA_CRAY; do</code></pre>
<pre><code>for MAALI_CRAY_MODULE in $MAALI_PREREQ_MODULE; do</code></pre>

*MAALI_EXTRA_CRAY* - is string variable that is a list of ???
<pre><code>for MAALI_CRAY_MODULE in $MAALI_EXTRA_CRAY; do</code></pre>

*MAALI_CRAY_PRGENV* - is a string variable that represents the variable in for loop list used in the maali_module function and the main maali scrtip.
<pre><code>for MAALI_CRAY_PRGENV in $MAALI_TOOL_CRAY_PRGENV; do</code></pre>

*MAALI_CRAY_PRGENV_FOUND_MATCH* - is a boolean variable used in the maali module function
<pre><code> if [ $MAALI_CRAY_PRGENV_FOUND_MATCH -eq 0 ]; then</code></pre>

*MAALI_CRAY_PRGENV_NAME* - String variable to used to test which PrgEnv is set
<pre><code>MAALI_CRAY_PRGENV_NAME=`echo "$MAALI_CRAY_PRGENV" | cut -d '/' -f 1`

*MAALI_CRAY_PRGENV_TYPE* - a string variable set in loop used in the maali_module function
<pre><code>for MAALI_CRAY_PRGENV_TYPE in $MAALI_SUPPORTED_CRAY_PRGENVS; do</code></pre>

*MAALI_CRAY_TOOL_COMPILER* - a string variable using the a loop in the maali main function.
<pre><code>for MAALI_CRAY_TOOL_COMPILER in $MAALI_CRAY_TOOL_COMPILERS; do</code></pre>
 
*MAALI_CRAY_TOOL_COMPILERS* - is a list of Compilers used
<pre><code>MAALI_CRAY_TOOL_COMPILERS="$MAALI_TOOL_COMPILERS"</code></pre>

*MAALI_CRAY_TOOL_COMPILER_NAME* - is a string variable of the environment module with the version truncated.
<pre><code>MAALI_CRAY_TOOL_COMPILER_NAME=`echo "$MAALI_CRAY_TOOL_COMPILER" | cut -d '/' -f 1`</code></pre>

*MAALI_CUDA_BUILD* - is a boolean variable for using the CUDA libs set in the cygnet files.

*MAALI_CUDA_SUPPORT* - is a boolean variable used to test for CUDA support
<pre><code>MAALI_CUDA_SUPPORT=0  ( could you not use MAALI_CUDA_BUILD )</code></pre>

*MAALI_CYGNET_BUILD_FILE_EXT* - a string variable for cygnet file extension
<pre><code>MAALI_CYGNET_BUILD_FILE_EXT='cyg'</code></pre>
 
*MAALI_CYGNET_DIR_NAME* - string variable use to define directory for the cygnet files
<pre><code>MAALI_CYGNET_DIR_NAME='cygnet_files'</code></pre>

*MAALI_CYGNET_FILEPATH* - installation path of cygnet file 
<pre><code>MAALI_CYGNET_FILEPATH="$MAALI_FILES_PATH/$MAALI_TOOL_NAME.$MAALI_SYSTEM.$MAALI_CYGNET_BUILD_FILE_EXT"</code></pre>

*MAALI_CYGNET_FILESIZE* - boolean variable test to that file exists
<pre><code>MAALI_CYGNET_FILESIZE=$(stat -c%s "$MAALI_CYGNET_FILEPATH")</code></pre>

*MAALI_CYGNET_FILES_URL* - string variable defined the url for the maali-cygnet files
<pre><code>MAALI_CYGNET_FILES_URL="https://raw.githubusercontent.com/Pawseyops/maali-cygnet/master"</code></pre>

*MAALI_DEFAULT_26_PYTHON
*MAALI_DEFAULT_27_PYTHON
*MAALI_DEFAULT_CCE_COMPILERS

*MAALI_DEFAULT_COMPILER* - string variable used in loop in maali main
<pre><code>for MAALI_DEFAULT_COMPILER in $MAALI_DEFAULT_COMPILERS; do</code></pre>

*MAALI_DEFAULT_COMPILERS - list variable using in loop in maali main
<pre><code>for MAALI_DEFAULT_COMPILER in $MAALI_DEFAULT_COMPILERS; do</code></pre>

*MAALI_DEFAULT_COMPILER_NAME* - the compiler environment module name with version truncated.
<pre><code>MAALI_DEFAULT_COMPILER_NAME=`echo "$MAALI_DEFAULT_COMPILER" | cut -d '/' -f 1`</code></pre>

*MAALI_DEFAULT_CRAY_CCE_PRGENV* - not used set in comment

*MAALI_DEFAULT_CRAY_CRAY_PRGENV* - string variable defined in the main maali script
<pre><code> Does not appear to be used...???</code></pre>

*MAALI_DEFAULT_CRAY_GCC_PRGENV* - string variable defined in the main maali script.
<pre><code> Does not appear to be used...???</code></pre>

*MAALI_DEFAULT_CRAY_INTEL_PRGENV* - string variable defined in the main maali script.
<pre><code> Does not appear to be used...???</code></pre>

*MAALI_DEFAULT_CRAY_PRGENVS* - list of cray prgenv using in a for loop in main maali script

*MAALI_DEFAULT_GCC_COMPILERS* - string variable defined in the main maali script.
<pre><code>Does not appear to be used...???</code></pre>

*MAALI_DEFAULT_INTEL_COMPILERS* - string variable defined in the main maali script.
<pre><code>Does not appear to be used...???</code></pre>

*MAALI_DEFAULT_PRGENV* - string variable defined in the main maali function in a for loop.
<pre><code> for MAALI_DEFAULT_PRGENV in $MAALI_DEFAULT_CRAY_PRGENVS; do

*MAALI_DEFAULT_PRGENV_NAME* - module name with the version truncated used in the main maali script
 <pre><code> MAALI_DEFAULT_PRGENV_NAME=`echo "$MAALI_DEFAULT_PRGENV" | cut -d '/' -f 1`</code></pre> 

*MAALI_DEFAULT_PYTHON_COMPILER* - string variable used as the index value in a for loop set in the main maali script.
<pre><code>for MAALI_DEFAULT_PYTHON_COMPILER in $MAALI_DEFAULT_PYTHON; do</code></pre>

*MAALI_DEFAULT_PYTHON_COMPILER_VERSION* - 
<pre><code>MAALI_DEFAULT_PYTHON_COMPILER_VERSION=`echo "$MAALI_DEFAULT_PYTHON_COMPILER" | cut -d '/' -f 2`</code></pre>

*MAALI_DEFAULT_PYTHON_COMPILER_MAJOR_MINOR_VERSION* 
<pre><code>MAALI_DEFAULT_PYTHON_COMPILER_MAJOR_MINOR_VERSION=`echo "$MAALI_DEFAULT_PYTHON_COMPILER_VERSION" | cut -d '.' -f 1,2`</code></pre>

*MAALI_DEFAULT_SYSTEM_GCC* - string variable used to define the default version of the gnu compilers. Used in cygnet files when only compiling with the gnu compilers.
 
*MAALI_DIR* - string variable local to the maali_makedir function

*MAALI_DIR_PATH* - string variable local the maail_makedir function

*MAALI_DST_ARRAY* - string variable local to the maali_download function. -- should be changed to DST_ARRAY

*MAALI_DST_COUNT* - string variable local to the maali_download function. -- should be changed to COUNT

*MAALI_DST_FILESIZE* - string variable local to the maali unpack function check the filesize is not zero. -- should be changed to FILESIZE

*MAALI_EXIT_STATUS* - string variable local to the maali run function test the exit status of the command run. -- should be changed to EXIT_STATUS

## *MAALI_EXTRA_CRAY_REV* - string variable in maali - is unused needs to be removed.

## *MAALI_FILES* - string variable is undefined an un-used variable in 
<pre><code>MAALI_FILES_DIR="$HOME/.maali/$MAALI_OS/$MAALI_CYGNET_DIR_NAME $HOME/.maali/$MAALI_CYGNET_DIR_NAME $MAALI_FILES $MAALI_DIR/$MAALI_CYGNET_DIR_NAME ."</code></pre>

*MAALI_FILES_DIR* - string variable used to defined the list of directories to search for cygnet files -- should be renamed something like CYGNET_FILE_DIR

*MAALI_FILES_PATH* - string variable used to for loop as the index across the MAALI_FILES_DIR list -- should be changed to something more meaningful.

## *MAALI_FORCE_DOWNLOAD_FROM_GITHUB* - boolean variable default = 0 is not used not sure what is original purpose was or is?

*MAALI_GIT_DOWNLOAD_DIR* - string variable that is set the github repo path used in the maali_git_download function.

*MAALI_GIT_DOWNLOAD_FILE * - string variable that is set the file name used in the maali_git_download function.

*MAALI_GIT_DOWNLOAD_FILEPATH * - string variable that is made up of the MAALI_GIT_DOWNLOAD_DIR/\ MAALI_GIT_DOWNLOAD_FILE used in the maali_git_download function.

*MAALI_GIT_DOWNLOAD_FILESIZE * - string variable that is used to evaluate the if the wget was successful used in the maali_git_download function.

## MAALI_GROUP_BUILD* - boolean varible that is set with the -g flag on the command line, is not used!

*MAALI_IFS* - string variable used to store the IFS value used in the maali_makedir function

*MAALI_INDIVIDUAL_DST* - string variable used the maali_unpack function as a index variable in the main for loop over the MAALI_DST (what is DST?)

*MAALI_INSTALL_DATE* - string variable used for writing the date to the wiki files.

*MAALI_INSTALL_DIR* - string variable that uses "$MAALI_INSTALL_TOOLDIR/$MAALI_TOOL_VERSION" as defined in the cygnet files. This is the installation directory for the tool.

*MAALI_INSTALL_TOOLDIR* - string variable that uses "$MAALI_ROOT/$MAALI_COMPILER/$MAALI_TOOL_NAME" or "MAALI_INSTALL_TOOLDIR="$MAALI_APPS_DIR/$MAALI_COMPILER/$MAALI_TOOL_NAME", path where maali installs the tool

*MAALI_IS_LOADED* - string variable used to determine if maali is loaded.

*MAALI_LOADEDMODULES * - string variable that is a list of the loaded environment modules.
 
*MAALI_LOADED_CRAY_COMPILER* - string variable that used for testing if the cray compiler PrgEnv is loaded correctly.

*MAALI_LOADED_PRGENV* - string variable that defines which PrgEnv module is loaded.

*MAALI_LOG_DIR* - string variable that defines the path for the build log files.
 <pre><code>MAALI_LOG_DIR="$HOME/.maali/$MAALI_OS/logs"</code></pre>

*MAALI_LOG_PREFIX* - string variable that prepends the config file name to file. 
<pre><code> MAALI_LOG_PREFIX="$MAALI_CONFIG-"</code></pre>

*MAALI_MAKE_VERSION_DEFAULT* - string variable used in main maali function as a command line option -a

*MAALI_MODULE* - string variable using in main maali function as a loop index in test if maali is loaded.

*MAALI_MODULE_CONFLICT_CHECK* - variable used in maali_load_module used to test for module conflicts

*MAALI_MODULE_DIR* - string variable that defines the location for the modulefiles.

*MAALI_MODULE_MAX_VARIABLE* - string variable used in the maali_module function.
<pre><code> MAALI_MODULE_MAX_VARIABLE=${#MAALI_APP_HOME_NAME} </code></pre>

*MAALI_MODULE_MISSING* - string variable used the maali load module function to test if the module file exists.

*MAALI_MODULE_NAME* - string variable used in the maali for a conditional testing for loading and unloading modules.

## *MAALI_MODULE_PE_PKGCONFIG_CFLAGS* - string variable for module creation for the cray -- is defined but not used

## *MAALI_MODULE_PE_PKGCONFIG_LIBS* - string variable for module creation for the cray -- is defined but not used

## *MAALI_MODULE_PE_PKGCONFIG_NAME* - string variable for module creation for the cray -- is defined but not used

## *MAALI_MODULE_PREREQ* - string variable used to list any prerequisite modules to be added to the module. 

*MAALI_MODULE_VARIABLE* - string variable used in maali to as and used as a for loop index value

*MAALI_MODULE_VARIABLE_LIST_OFF* - string variable set in the maali module function 
<pre><code> MAALI_MODULE_VARIABLE_LIST_OFF="MAALI_MODULE_LIST_${MAALI_MODULE_VARIABLE_NAME}"</code></pre>

*MAALI_MODULE_VARIABLE_LIST_OFF_VALUE* - string variable set in the maali module function 
<pre><code> MAALI_MODULE_VARIABLE_LIST_OFF_VALUE=`eval echo "${!MAALI_MODULE_VARIABLE_LIST_OFF}"`</code></pre>

*MAALI_MODULE_VARIABLES_LIST* - string variable set in the maali module function lists all the variables which start with *MAALI_MODULE_SET_*.
<pre><code> MAALI_MODULE_VARIABLES_LIST="$MAALI_MODULE_VARIABLES_LIST_MAALI $MAALI_MODULE_VARIABLES_LIST_NON_MAALI </code></pre>

*MAALI_MODULE_VARIABLES_LIST_MAALI* - string variable used in the maali module function for creating the python module.

*MAALI_MODULE_VARIABLES_LIST_NON_MAALI* - string variable used in the maali module function for creating the python module.

*MAALI_MODULE_VARIABLE_DIR* - string variable used in maali module function used to define the different variable paths when creating the module.

*MAALI_MODULE_VARIABLE_EVAL* - string variable using in maali module function.
<pre><code>MAALI_MODULE_VARIABLE_EVAL=`eval echo "$MAALI_MODULE_VARIABLE_VALUE"`</code></pre>

*MAALI_MODULE_VARIABLE_VALUE* - string variable used in the maali module function as an index variable in a for loop.

*MAALI_MODULE_VARIABLE_NAME* - string variable using the maali module function. 
<pre><code> MAALI_MODULE_VARIABLE_NAME=`echo $MAALI_MODULE_VARIABLE | sed -e 's/MAALI_MODULE_SET_//g'` </code></pre>

*MAALI_MODULE_VARIABLE_PREPREND* - string variable used in the maali module function to set installation path based on tool type. 
<pre><code> MAALI_MODULE_VARIABLE_PREPREND="$MAALI_APP_HOME/" </code></pre>

*MAALI_MODULE_VARIABLE_TCL* - string variable used in the maali module function to prepend-path in module.

*MAALI_MODULE_VARIABLE_WIDTH* - variable used in the maali module function to find the width of name for printf statements writing the module file.

## *MAALI_MODULE_VARIABLE_WORDS* - variable using in maali module function that does ???

*MAALI_NEWPYTHONPATH* - string variable that is set in the maali_python_build function 
<pre><code>MAALI_NEWPYTHONPATH="$MAALI_INSTALL_DIR/$MAALI_PYTHON_LIBDIR/python$MAALI_PYTHON_LIB_VERSION/site-packages"</code></pre>

*MAALI_OLD_PYTHONPATH* - string variable that is used a temporary variable in the maali_python_build function.

*MAALI_OS* - a string variable that defines what the OS or linux image that is install. Detected automatically by maali by check
contents of files in /etc. MAALI_OS is part of the path for the location for the cygnet files.

*MAALI_OS_PATCH_LEVEL* - variable that set to the correct service package level for determing the MAALI_OS
<pre><code> MAALI_OS_PATCH_LEVEL=`grep PATCHLEVEL /etc/SuSE-release | cut -d '=' -f 2 | sed -e 's/ //g'`</code></pre>

*MAALI_OS_VERSION* - variable that is set to ther release verion of the OS used for seting MAALI_OS
<pre><code>MAALI_OS_VERSION=`grep VERSION_ID /etc/os-release | cut -d '=' -f 2 | sed -e 's/"//g'`</code></pre>

*MAALI_PATCH_NUMBER* - variable that is used as a loop index used in downloading patch files.

*MAALI_PKG* - string variable that is used as a loop index for construcing various lists for writing environment modules.

*MAALI_PREREQ_MODULE_NAME* - string variable that is set to the name of the module without the version.
<pre><code>MAALI_PREREQ_MODULE_NAME=`echo "$MAALI_PREREQ_MODULE" | cut -d '/' -f 1`</code></pre>

## MAALI_PYTHON_FIRSTLETTER* - string variable that is unused.
<pre><code>MAALI_PYTHON_FIRSTLETTER=`echo "$MAALI_TOOL_NAME_ORIG" | cut -b 1`</code></pre>

*MAALI_PYTHON_LIB_VERSION* - variable that is set to the major release of the python lib ie. 2.6, 2.7, 3.2 etc.
<pre><code>MAALI_PYTHON_LIB_VERSION=`echo "$MAALI_PYTHON_VERSION" | cut -d '.' -f 1,2`</code></pre>

*MAALI_PYTHON_TOOL* - variable set if we are using building with python.

*MAALI_PYTHON_VERSION* - variable that is set to which version of python is being used.

*MAALI_REBUILD* - boolean variable that is set when using the maali usage flags.

*MAALI_REBUILD_COMPILER* - string variable that is set when using the maali usage file -r allows user to specify which compiler they wish to use.

## *MAALI_REBUILD_COMPILER_LOG* - string variable that used when write log file.

*MAALI_REBUILD_SRC* - string variable that shows the path for recording logs of rebuilds
<pre><code>MAALI_REBUILD_SRC="$HOME/.maali/$MAALI_OS/rebuild"</code></pre>

*MAALI_REMODULE* - boolean variable that is set when using the maali usage flage to only update the module file.

## MAALI_REQUIRED_SYSTEM_MODULES* - string variable that is a list of required system modules but is not set anywhere and there is section dead code.

*MAALI_ROOT* - string variable that is the root path for detemining where things are installed using maali.

*MAALI_R_TOOL* - usage flag boolean variables to build R applications.  

*MAALI_SLASH_COUNT* - variable used to test the correctness of the compilers when doing binary package installs

*MAALI_SRC* - string variable set to define the full path to where the application/tool source tar ball files are saved.

*MAALI_SUPPORTED_COMPILERS* - string variable set in the maali_module function 
<pre><code> MAALI_SUPPORTED_COMPILERS="gcc intel cce pgi" # note it is gcc not gnu</code></pre>

*MAALI_SUPPORTED_CRAY_CPU_TARGETS* - string variable set in the maali_module function listing the cpu types
<pre><code>MAALI_SUPPORTED_CRAY_CPU_TARGETS="sandybridge ivybridge haswell"</code></pre>

*MAALI_SUPPORTED_CRAY_PRGENVS* - string variable set in the maali_module function listing the different PrgEnv-*
<pre><code>MAALI_SUPPORTED_CRAY_PRGENVS="PrgEnv-gnu PrgEnv-intel PrgEnv-cray"</code></pre>

*MAALI_SYSTEM* - string variable defined in the config file 

*MAALI_SYSTEM_BUILD* - Boolean variable to determine if build will be a system build otherwise it is a group/project/user build.

*MAALI_SYSTEM_COMPILER* - string variable used as a index variable in a for loop
 <pre><code>for MAALI_SYSTEM_COMPILER in $MAALI_SYSTEM_COMPILER_LIST; do</code></pre>

*MAALI_DEFAULT_SYSTEM_GCC* - string variable defined in config file and used in cygnet files when only building with the default gcc/gnu compilers

*MAALI_SYSTEM_COMPILER_LIST* - string variable defining list of installed compilers without versioning
<pre><code>MAALI_SYSTEM_COMPILER_LIST=`module avail $MAALI_COMPILER_TYPE/ -t 2>&1 | grep $MAALI_COMPILER_TYPE/ | sed -e 's/(default)//g'`</code></pre>

*MAALI_SYSTEM_CRAY_CPU_TARGET* - string variable used as an index variable for MAALI_SYSTEM_CRAY_CPU_TARGET_LIST

*MAALI_SYSTEM_CRAY_CPU_TARGET_LIST* - string variable defines list of modules "craype-*"

*MAALI_SYSTEM_CRAY_PRGENV* - string variable using as and index variable in a bash for loop.

*MAALI_SYSTEM_CRAY_PRGENV_LIST* - string variable that is a list of the Cray PrgEnv modules.

*MAALI_SYSTEM_CUDA_LIST* - string variable that is a list of the CUDA modules.

*MAALI_SYSTEM_CUDA_VERSION* - string variable using as and index variable in a bash for loop.

*MAALI_SYSTEM_CUDA_VERSION_MATCH* - string variable used to test if CUDA is installed

*MAALI_SYSTEM_CYGNET_FILE* - boolean varible used to determing correct application home 
<pre><code> if [ $MAALI_SYSTEM_CYGNET_FILE* -eq 1 ]; then</code></pre>

*MAALI_SYSTEM_DEFAULT_CUDA* - variable is set to determine list version of cuda installled on system.

*MAALI_SYSTEM_DEFAULT_PYTHON* - variable is set to determine list version of Python installed on system.

*MAALI_SYSTEM_DEFAULT_R * - variable is set to determine list version of R installed on system.

*MAALI_SYSTEM_PACKAGES_NAME* - string variable used as a variable index in a bash for loop.
<pre><code>for MAALI_SYSTEM_PACKAGES_NAME in $MAALI_SYSTEM_PACKAGES_PREREQ; do</code></pre>

*MAALI_SYSTEM_PACKAGES_PREREQ* - is an undefined string variable
<pre><code>for MAALI_SYSTEM_PACKAGES_NAME in $MAALI_SYSTEM_PACKAGES_PREREQ; do</code></pre>

*MAALI_SYSTEM_PACKAGES_VERSION
<pre><code>MAALI_SYSTEM_PACKAGES_VERSION=`rpmquery* --qf "%{VERSION}" ${MAALI_SYSTEM_PACKAGES_NAME}`</code></pre>

*MAALI_TMP_DST* - string variable used as an index variable in a bash for loop in the maali_download function
<pre><code>  for MAALI_TMP_DST in $MAALI_DST ; do</code></pre>

*MAALI_TMP_URL* - string variable used as an index variable in a bash for loop in the maali_download function
<pre><code>  for MAALI_TMP_URL in $MAALI_URL ; do</code></pre>

*MAALI_TMP_FILE* - temporary assigned string variable used in the maali_load_module function 
<pre><code> MAALI_TMP_FILE="$TMPDIR/maali.$$.out"</code></pre>

*MAALI_TOOL_BUILD_PREREQ_REV* - string variable that has a formated list of prerequisite module with a "space" as a seperator.
<pre><code>MAALI_TOOL_BUILD_PREREQ_REV=`echo $MAALI_TOOL_BUILD_PREREQ | tac* -s' '`</code></pre>

*MAALI_TOOL_CRAY_CPU_TARGET* - string variable used in the maali_module function
<pre><code>MAALI_TOOL_CRAY_CPU_TARGET=`echo $MAALI_PREREQ_MODULE_NAME | cut* -d '-'* -f 2`</code></pre>
 
*MAALI_TOOL_CUDA_COMPILER* - string variable that is used as the index variable in bash for loop.
<pre><code>for MAALI_TOOL_CUDA_COMPILER in $MAALI_TOOL_CUDA_COMPILERS; do</code></pre>

*MAALI_TOOL_CUDA_COMPILERS* - string variable that is set in the cygnet files, used to define the list of CUDA compilers.

*MAALI_TOOL_LOG* - string variable that defines the path and file name for build log
<pre><code>MAALI_TOOL_LOG="$MAALI_LOG_DIR/$MAALI_LOG_PREFIX$MAALI_TOOL_NAME-$MAALI_TOOL_VERSION.log"</code></pre>

*MAALI_TOOL_MAJOR_MINOR_VERSION* - variable that is set to the version number of the application
<pre><code>MAALI_TOOL_MAJOR_MINOR_VERSION=`echo "$MAALI_TOOL_VERSION" | cut* -d '.'* -f 1,2`</code></pre>

*MAALI_TOOL_MAJOR_VERSION* - variable that is set to the point release version of the application
<pre><code>MAALI_TOOL_MAJOR_VERSION=`echo "$MAALI_TOOL_VERSION" | cut* -d '.'* -f 1`</code></pre>

## *MAALI_TOOL_NAME-gpu* - string variable that is set to the name of the application/lib the "-gpu" is appended to the module name if it is gpu enabled.
variable should be \${MAALI_TOOL_NAME}-gpu for clarity and ensure consistency

*MAALI_TOOL_MODULE_DIR* - string variable that is set to the full path of directory 
<pre><code>MAALI_TOOL_MODULE_DIR="$MAALI_MODULE_DIR/$MAALI_TOOL_NAME"</code></pre>

*MAALI_TOOL_MODULE* - string variable that is set to the directory and file for the module file.
<pre><code>MAALI_TOOL_MODULE="$MAALI_TOOL_MODULE_DIR/$MAALI_TOOL_VERSION"</code></pre>

*MAALI_TOOL_NAME_ORIG* - string variable that holds the original name with upper case letters
<pre><code>MAALI_TOOL_NAME_ORIG=$MAALI_TOOL_NAME</code></pre>

*MAALI_TOOL_NAME_UPPERCASE* - string variable with MAALI_TOOL_NAME in uppercase for use in the maali_module function 
<pre><code>MAALI_TOOL_NAME_UPPERCASE=$(echo "$MAALI_TOOL_NAME" | tr '[:lower:]' '[:upper:]' | sed* -e 's/-//g' | sed* -e 's/+/PLUS/g' )</code></pre>

*MAALI_TOOL_PREREQ_MODULE* - string variable used in the maali_module function to evaluate which compiler suite is loaded.
<pre><code>MAALI_PREREQ_MODULE_NAME=`echo "$MAALI_PREREQ_MODULE" | cut* -d '/'* -f 1`</code></pre>

*MAALI_TOOL_PREREQ_REV* - string variable that creates a formatted list of the prerequisite modules.
<pre><code>MAALI_TOOL_PREREQ_REV=`echo $MAALI_TOOL_PREREQ | tac* -s' '`</code></pre>

*MAALI_TOOL_VERSION_CURRENT_DEFAULT* - variable used to count number of versions of a module
<pre><code> MAALI_TOOL_VERSION_CURRENT_DEFAULT=`grep \"$MAALI_TOOL_VERSION\" $MAALI_TOOL_MODULE_DIR/.version | wc -l`</code></pre>

*MAALI_TYPE* - string variable that represents the type of file compression used.
<pre><code>MAALI_TYPE=`file -b $MAALI_INDIVIDUAL_DST | cut -d ',' -f 1`</code></pre>

*MAALI_UNINSTALL_HIDE* - boolean variable used hide the module file maali usage option -u 

*MAALI_UNINSTALL_REMOVE* - boolean variable used to remove file "only removes log files and modules"
 

*MAALI_URL_ARRAY* - is an array of string variables that define multiple URLS for an application if needed
<pre><code>MAALI_URL_ARRAY[$i]=$MAALI_TMP_URL</code></pre>

*MAALI_USER_MAP* - is defined for system config files to help track system build files.

*MAALI_VERSION* - variable that defines what version of maali is being used.

*MAALI_WGET_CMD* - is a string variable that determine wget flags are included to the wget command.

*MAALI_WGET_EXTRA* - is a string variable that defines optional flags for the wget command

*MAALI_WIKI_CATEGORY_ITEM* - string variable defined as an index variable for a bash for loop
<pre><code>for MAALI_WIKI_CATEGORY_ITEM in $MAALI_WIKI_CATEGORY; do</code></pre>

*MAALI_WIKI_COMPILER_STRING* - string variable used to write out to wiki the MAALI_COMPILER_VERSION value 
<pre><code>MAALI_WIKI_COMPILER_STRING="<tt>$MAALI_COMPILER_VERSION</tt>"</code></pre>

*MAALI_WIKI_SRC* - string variable that is the full path to folder with the wiki file 
<pre><code>MAALI_WIKI_SRC="$HOME/.maali/$MAALI_OS/wiki/$MAALI_LOG_PREFIX$MAALI_TOOL_NAME-$MAALI_TOOL_VERSION.wiki"</code></pre>

*MAALI_WIKI_SYSTEM_NAME* - format MAALI_SYSTEM variable to use in writing wiki file.
<pre><code>MAALI_WIKI_SYSTEM_NAME=`echo ${MAALI_SYSTEM:0:1} | tr  '[a-z]' '[A-Z]'`${MAALI_SYSTEM:1}</code></pre>

*MAALI_WIKI_TOOL_NAME* - string variable that is set to the MAALI_TOOL_NAME_ORIG value
<pre><code>MAALI_WIKI_TOOL_NAME="$MAALI_TOOL_NAME_ORIG"</code></pre>

