# Fax -- Insatll
* Install Java
* Install Ant
* Install Python 2.7
* Fax requires that the Z3 dynamic link library (libz3.so on Unix-like platforms) as well as the dynamic link library for the Z3/Java API (libz3java.so on Unix-like platforms) be in your dynamic library path ($LD\_LIBRARY\_PATH on Unix-like platforms; just PATH on Windows).
  * For Windows: copy lib/libz3/libz3.dll and lib/libz3/libz3java.dll to %JAVA_HOME%\bin\.
  * For Unix-like platforms: 
    * add java.library.path into file /etc/profile (export LD\_LIBRARY\_PATH=$LD\_LIBRARY\_PATH:/[your\_library\_path]).
    * copy lib/libz3/libz3.so and lib/libz3/libz3java.so to [your\_library\_path]
    * source /etc/profile
* Prepare Android environment, the version of Android SDK is 17 and the version of Android SDK Tools should lower than 25.2.3.
* Run "java -version", "python", "ant -version", "android create project" to check whether these tools are successfully configured.
* For quick start, you can run: runFax.sh or runFax.bat
* Install Java
* Install Ant
* Install Python 2.7
* Fax requires the Z3 dynamic link library as well as the dynamic link library for the Z3/Java API .
  * For Windows: copy lib/libz/libz3.dll and lib/libz3/libz3java.dll to %JAVA_HOME%\bin\.
  * For Unix-like platforms: 
    * copy lib/libz3/libz3.so and lib/libz3/libz3java.so to [your\_library\_path]
	 * add java.library.path into file "/etc/profile" (export LD\_LIBRARY\_PATH=$LD\_LIBRARY\_PATH:/[your\_library_path]).
    * save the modification (source /etc/profile)
* Prepare Android environment, the version of Android SDK is 17 and the version of Android SDK Tools should lower than 25.2.3.
* Run "java -version", "python", "ant -version", "android create project" to check whether these tools are successfully configured.

## Usage of Fax:
* Prepare an Android device or emulator and connect it to you computer. Turn the setting item "developer mode" on and allow "usb debugging".  
* Clone the project to your computer. 

* **For quick start, you can run: runFax.sh or runFax.bat**  
	*To modify the exploration time of Fax\_ex, please update the variable exploration_time in file FaxTool\script\run.py.   
	In our paper, the exploration time is one hour (3600). If you want a quick validation, use the default value in Fax (60).  

* Preprocess
Put the apk under test under the [apk_input_dir], and get the result in the [apk_output_dir].  
python  script/Prerocess.py  [apk_input_dir]  [apk_output_dir]  
e.g., python  script/Prerocess.py  apk  apk_processed  
Then the activities will be exposed, the app will be instrumented, and then installed on the device.  
For log-in required apps, you can log-in first after installation to improve the exploration coverage.    


* Test Generation
python  script/GenerateTestCases.py  lib/Fax0108.jar [apk_input_dir]  [testcase_dir] [execute_info_dir] [max_number_of_path]  
e.g., python  script/GenerateTestCases.py  lib/Fax0108.jar  apk Result_testGen  executeInfo 10000  
Get results in folder [testcase_dir].

* Fax Launching Test
python script/LaunchActs.py [testcase_dir]  [launch_dir]  
e.g., python script/LaunchActs.py Result_testGen Result_launch  
Get results in folder [launch_dir].

* Detect Crashes for Fax Launching
python script/FindCrashes.py [testcase_dir]  [launch_dir]   
e.g., python script/FindCrashes.py Result_testGen Result_launch  

* Fax Exploration Test
python script/FairAndroidExploer.py [launch_dir]  [explore_dir]  [testcase_dir] [apk_output_dir] [EA/All]  
Set EA if you want to set only EA as the exploration entry.   
Set All if you want to set all activities as the exploration entry.   
e.g., python script/FairAndroidExploer.py Result_launch Result_explore Result_testGen apk_processed ALL  
Get results in folder [explore_dir].

* Detect Crashes for Fax Exploration
python script/FindCrashes.py [testcase_dir]  [explore_dir]   
e.g., python script/FindCrashes.py Result_testGen  Result_explore  

* Calculate Coverage Result
python script/CollectLogcat.py  [testcase_dir]  [explore_dir]  [apk_output_dir]  
e.g., python script/CollectLogcat.py  Result_testGen  Result_explore apk_processed  