@Mrwatchara8
ภาษาหลักภายในอุปกรณ์ลงทะเบียน
เปลี่ยนภาษา: 
อังกฤษ
การใช้ PHP จากบรรทัดคําสั่ง ¶
สารบัญ ¶
ความแตกต่างกับ SAPI อื่น ๆ
ตัวเลือก
การใช้
สตรีม I/O
เปลือกแบบโต้ตอบ
เว็บเซิร์ฟเวอร์ในตัว
การตั้งค่า INI
แนะ นำ ¶
จุดสนใจหลักของ CLI SAPI คือการพัฒนาแอปพลิเคชันเชลล์ด้วย PHP ที่นั่น มีความแตกต่างเล็กน้อยระหว่าง CLI SAPI และ SAPIอื่น ๆ ซึ่งอธิบายไว้ในบทนี้ มันคุ้มค่า กล่าวถึง CLI และ CGI เป็น SAPIที่แตกต่างกัน แม้ว่าจะมีพฤติกรรมเหมือนกันหลายอย่าง

CLI SAPI ถูกเปิดใช้งานโดยค่าเริ่มต้นโดยใช้ --enable-cli แต่อาจปิดใช้งานโดยใช้ --disable-cli เมื่อเรียกใช้ ./configure

ชื่อ ตําแหน่ง และการมีอยู่ของไบนารี CLI/CGI จะแตกต่างกันไปขึ้นอยู่กับวิธีการติดตั้ง PHP ในระบบของคุณ โดย default เมื่อดําเนินการ make ทั้ง CGI และ CLI จะถูกสร้างขึ้นและวางไว้ตามลําดับในไดเร็กทอรีต้นทาง PHP ของคุณ คุณจะสังเกตว่าทั้งสองมีชื่อว่า . สิ่งที่เกิดขึ้นระหว่างการติดตั้งขึ้นอยู่กับบรรทัดการกําหนดค่าของคุณ หากเลือกโมดูล SAPI ในระหว่างการกําหนดค่า เช่น apxs หรือใช้ตัวเลือก --disable-cgi CLI คือ คัดลอกไปยังระหว่างทําการติดตั้ง มิฉะนั้น CGI จะถูกวาง ที่นั่น ตัวอย่างเช่น ถ้า --with-apxs เป็น ในบรรทัดกําหนดค่าของคุณ จากนั้น CLI จะถูกคัดลอกไประหว่างทําการติดตั้ง หากคุณต้องการแทนที่ การติดตั้งไบนารี CGI ใช้ make install-cli หลังจากทําการติดตั้ง อีกทางหนึ่งคุณ สามารถระบุ --disable-cgi ในการกําหนดค่าของคุณ สาย sapi/cgi/php-cgisapi/cli/phpphp{PREFIX}/bin/php{PREFIX}/bin/php

หมายเหตุ

เนื่องจากทั้ง --enable-cli และ --enable-cgi ถูกเปิดใช้งานโดยค่าเริ่มต้น เพียงแค่มี --enable-cli ใน configure line ไม่จําเป็นต้องหมายความว่า CLI จะถูกคัดลอกเป็นระหว่างทําการติดตั้ง {PREFIX}/bin/php

ไบนารี CLI แจกจ่ายในโฟลเดอร์หลักเช่นเดียวกับใน Windows เวอร์ชัน CGI คือ แจกจ่ายเป็น . นอกจากนี้ a จะถูกแจกจ่ายหากกําหนดค่า PHP โดยใช้ --enable-cli-win32 สิ่งนี้ทําเหมือนกับ เวอร์ชัน CLI ยกเว้นว่ามันไม่ได้ส่งออกอะไรเลยและด้วยเหตุนี้จึงให้ ไม่มีคอนโซล php.exephp-cgi.exe php-win.exe

หมายเหตุ: ฉันมี SAPI อะไรบ้าง?

จากเชลล์พิมพ์ php -v จะบอกคุณ ไม่ว่าจะเป็น CGI หรือ CLI เห็น ฟังก์ชัน php_sapi_name() และค่าคงที่ PHP_SAPI php

หมายเหตุ

หน้า Unix ual สามารถใช้ได้โดยพิมพ์ man PHP ในสภาพแวดล้อมเชลล์ man

พบปัญหา?
เรียนรู้วิธีปรับปรุงหน้านี้ • ส่งคําขอดึงข้อมูล • รายงานข้อบกพร่อง
+เพิ่มโน้ต
บันทึกที่ผู้ใช้มีส่วนร่วม 33 หมายเหตุ
ขึ้น
ลง
120ก.ย. 16 ที่ PSU DOT edu ¶13 ปีที่แล้ว
You can easily parse command line arguments into the $_GET variable by using the parse_str() function.

<?php

parse_str(implode('&', array_slice($argv, 1)), $_GET);

?>

It behaves exactly like you'd expect with cgi-php.

$ php -f somefile.php a=1 b[]=2 b[]=3

This will set $_GET['a'] to '1' and $_GET['b'] to array('2', '3').

Even better, instead of putting that line in every file, take advantage of PHP's auto_prepend_file directive.  Put that line in its own file and set the auto_prepend_file directive in your cli-specific php.ini like so:

auto_prepend_file = "/etc/php/cli-php5.3/local.prepend.php"

It will be automatically prepended to any PHP file run from the command line.
ขึ้น
ลง
28OHCC ที่ 163 ดอทคอม ¶8 ปีที่แล้ว
use " instead of ' on windows when using the cli version with -r

php -r "echo 1" 
-- correct

php -r 'echo 1'
  PHP Parse error:  syntax error, unexpected ''echo' (T_ENCAPSED_AND_WHITESPACE), expecting end of file in Command line code on line 1
ขึ้น
ลง
22PSIKYO ที่จดหมาย dot dlut dot edu dot cn ¶11 ปีที่แล้ว
If you edit a php file in windows, upload and run it on linux with command line method. You may encounter a running problem probably like that:

[root@ItsCloud02 wsdl]# ./lnxcli.php
Extension './lnxcli.php' not present.

Or you may encounter some other strange problem.
Care the enter key. In windows environment, enter key generate two binary characters '0D0A'. But in Linux, enter key generate just only a 'OA'.
I wish it can help someone if you are using windows to code php and run it as a command line program on linux.
ขึ้น
ลง
25apmuthu ที่ usa dot net ¶7 ปีที่แล้ว
Adding a pause() function to PHP waiting for any user input returning it:

<?php
function pause() {
    $handle = fopen ("php://stdin","r");
    do { $line = fgets($handle); } while ($line == '');
    fclose($handle);
    return $line;
}
?>
up
down
23Anonymous ¶4 years ago
We can pass many arguments directly into the hashbang line.
As example many ini setting via the -d parameter of php.
---
#!/usr/bin/php -d memory_limit=2048M -d post_max_size=0
phpinfo();
exit;
---
./script | grep memory
memory_limit => 2048M => 2048M
---
But we can also use this behaviour into a second script, so it call the first as an interpreter, via the hashbang:
---
#!./script arg1 arg2 arg3 
---
However the parameters are dispatched in a different way into $argv

All the parameters are in $argv[1], $argv[0] is the interpreter script name, and $argv[1] is the caller script name.

To get back the parameters into $argv, we can simply test if $argv[1] contains spaces, and then dispatch again as normal: 

#!/usr/bin/php -d memory_limit=2048M -d post_max_size=0
<?php
var_dump($argv);
if (strpos($argv[1], ' ') !== false){
  $argw = explode(" ", $argv[1]);
  array_unshift($argw, $argv[2]);
  $argv = $argw;
}
var_dump($argv); ?>
---
array(3) {
  [0]=>
  string(8) "./script"
  [1]=>
  string(15) "arg1 arg2 arg3 "
  [2]=>
  string(14) "./other_script"
}
array(4) {
  [0]=>
  string(8) "./other_script"
  [1]=>
  string(4) "arg1"
  [2]=>
  string(4) "arg2"
  [3]=>
  string(4) "arg3"
}
---
This will maintain the same behaviour in all cases and allow to even double click a script to call both parameters of another script, and even make a full interpreter language layer.  The other script doesn't has to be php. Take care of paths.
up
down
26frankNospamwanted at. toppoint dot. de ¶10 years ago
Parsing commandline argument GET String without changing the PHP script (linux shell):
URL: index.php?a=1&b=2
Result: output.html

echo "" | php -R 'include("index.php");' -B 'parse_str($argv[1], $_GET);' 'a=1&b=2' >output.html

(no need to change php.ini)

You can put this 
  echo "" | php -R 'include("'$1'");' -B 'parse_str($argv[1], $_GET);' "$2"
in a bash script "php_get" to use it like this:
  php_get index.php 'a=1&b=2' >output.html
or directed to text browser...
  php_get index.php 'a=1&b=2' |w3m -T text/html
up
down
24drewish at katherinehouse dot com ¶19 years ago
When you're writing one line php scripts remember that 'php://stdin' is your friend. Here's a simple program I use to format PHP code for inclusion on my blog:

UNIX:
  cat test.php | php -r "print htmlentities(file_get_contents('php://stdin'));"

DOS/Windows:
  type test.php | php -r "print htmlentities(file_get_contents('php://stdin'));"
up
down
33ben at slax0rnet dot com ¶21 years ago
Just a note for people trying to use interactive mode from the commandline.

The purpose of interactive mode is to parse code snippits without actually leaving php, and it works like this:

[root@localhost php-4.3.4]# php -a
Interactive mode enabled

<?php echo "hi!"; ?>
<note, here we would press CTRL-D to parse everything we've entered so far>
hi!
<?php exit(); ?>
<ctrl-d here again>
[root@localhost php-4.3.4]#

I noticed this somehow got ommited from the docs, hope it helps someone!
up
down
25Kodeart ¶13 years ago
Check directly without calling functions:

<?php

if (PHP_SAPI === 'cli')

{

   // ...

}

?>



You can define a constant to use it elsewhere

<?php

define('ISCLI', PHP_SAPI === 'cli');

?>
ขึ้น
ลง
27notreallyanaddress ที่ somerandomaddr dot com ¶15 ปีที่แล้ว
If you want to be interactive with the user and accept user input, all you need to do is read from stdin.  

<?php
echo "Are you sure you want to do this?  Type 'yes' to continue: ";
$handle = fopen ("php://stdin","r");
$line = fgets($handle);
if(trim($line) != 'yes'){
    echo "ABORTING!\n";
    exit;
}
echo "\n";
echo "Thank you, continuing...\n";
?>
ขึ้น
ลง
14phpnotes ที่ ssilk dot de ¶22 ปีที่แล้ว
To hand over the GET-variables in interactive mode like in HTTP-Mode (e.g. your URI is myprog.html?hugo=bla&bla=hugo), you have to call

php myprog.html '&hugo=bla&bla=hugo'

(two & instead of ? and &!)

There just a little difference in the $ARGC, $ARGV values, but I think this is in those cases not relevant.
ขึ้น
ลง
20OverFlow636 ที่ gmail dot com ¶19 ปีที่แล้ว
I needed this, you proly wont tho.

puts the exicution args into $_GET

<?php

if ($argv) {

    foreach ($argv as $k=>$v)

    {

        if ($k==0) continue;

        $it = explode("=",$argv[$i]);

        if (isset($it[1])) $_GET[$it[0]] = $it[1];

    }

}

?>
ขึ้น
ลง
17Lucas dot vasconcelos ที่ gmail dot com ¶17 ปีที่แล้ว
Just another variant of previous script that group arguments doesn't starts with '-' or '--'



<?php

function arguments($argv) {

    $_ARG = array();

    foreach ($argv as $arg) {

      if (ereg('--([^=]+)=(.*)',$arg,$reg)) {

        $_ARG[$reg[1]] = $reg[2];

      } elseif(ereg('^-([a-zA-Z0-9])',$arg,$reg)) {

            $_ARG[$reg[1]] = 'true';

      } else {

            $_ARG['input'][]=$arg;

      }

    }

  return $_ARG;

}



print_r(arguments($argv));

?>



$ php myscript.php --user=nobody /etc/apache2/*

Array

(

    [input] => Array

        (

            [0] => myscript.php

            [1] => /etc/apache2/apache2.conf

            [2] => /etc/apache2/conf.d

            [3] => /etc/apache2/envvars

            [4] => /etc/apache2/httpd.conf

            [5] => /etc/apache2/mods-available

            [6] => /etc/apache2/mods-enabled

            [7] => /etc/apache2/ports.conf

            [8] => /etc/apache2/sites-available

            [9] => /etc/apache2/sites-enabled

        )



    [user] => nobody

)
up
down
15roberto dot dimas at gmail dot com ¶20 years ago
One of the things I like about perl and vbscripts, is the fact that I can name a file e.g. 'test.pl' and just have to type 'test, without the .pl extension' on the windows command line and the command processor knows that it is a perl file and executes it using the perl command interpreter.

I did the same with the file extension .php3 (I will use php3 exclusivelly for command line php scripts, I'm doing this because my text editor VIM 6.3 already has the correct syntax highlighting for .php3 files ).

I modified the PATHEXT environment variable in Windows XP, from the " 'system' control panel applet->'Advanced' tab->'Environment Variables' button-> 'System variables' text area".

Then from control panel "Folder Options" applet-> 'File Types' tab, I added a new file extention (php3), using the button 'New'  and typing php3 in the window that pops up.

Then in the 'Details for php3 extention' area I used the 'Change' button to look for the Php.exe executable so that the php3 file extentions are associated with the php executable.

You have to modify also the 'PATH' environment variable, pointing to the folder where the php executable is installed

Hope this is useful to somebody
up
down
16jeff at noSpam[] dot genhex dot net ¶22 years ago
You can also call the script from the command line after chmod'ing the file (ie: chmod 755 file.php).

On your first line of the file, enter "#!/usr/bin/php" (or to wherever your php executable is located).  If you want to suppress the PHP headers, use the line of "#!/usr/bin/php -q" for your path.
up
down
16Anonymous ¶14 years ago
Using CLI (on WIN at least), some INI paths are relative to the current working directory.  For example, if your error_log = "php_errors.log", then php_errors.log will be created (or appended to if already exists) in whatever directory you happen to be in at the moment if you have write access there.  Instead of having random error logs all over the place because of this behavior, you may want to set error_log to a full path, perhaps to the php.exe directory.
up
down
22thomas dot harding at laposte dot net ¶17 years ago
Parsing command line: optimization is evil!

One thing all contributors on this page forgotten is that you can suround an argv with single or double quotes. So the join coupled together with the preg_match_all will always break that :)

Here is a proposal:

#!/usr/bin/php
<?php
print_r(arguments($argv));

function arguments ( $args )
{
  array_shift( $args );
  $endofoptions = false;

  $ret = array
    (
    'commands' => array(),
    'options' => array(),
    'flags'    => array(),
    'arguments' => array(),
    );

  while ( $arg = array_shift($args) )
  {

    // if we have reached end of options,
    //we cast all remaining argvs as arguments
    if ($endofoptions)
    {
      $ret['arguments'][] = $arg;
      continue;
    }

    // Is it a command? (prefixed with --)
    if ( substr( $arg, 0, 2 ) === '--' )
    {

      // is it the end of options flag?
      if (!isset ($arg[3]))
      {
        $endofoptions = true;; // end of options;
        continue;
      }

      $value = "";
      $com   = substr( $arg, 2 );

      // is it the syntax '--option=argument'?
      if (strpos($com,'='))
        list($com,$value) = split("=",$com,2);

      // is the option not followed by another option but by arguments
      elseif (strpos($args[0],'-') !== 0)
      {
        while (strpos($args[0],'-') !== 0)
          $value .= array_shift($args).' ';
        $value = rtrim($value,' ');
      }

      $ret['options'][$com] = !empty($value) ? $value : true;
      continue;

    }

    // Is it a flag or a serial of flags? (prefixed with -)
    if ( substr( $arg, 0, 1 ) === '-' )
    {
      for ($i = 1; isset($arg[$i]) ; $i++)
        $ret['flags'][] = $arg[$i];
      continue;
    }

    // finally, it is not option, nor flag, nor argument
    $ret['commands'][] = $arg;
    continue;
  }

  if (!count($ret['options']) && !count($ret['flags']))
  {
    $ret['arguments'] = array_merge($ret['commands'], $ret['arguments']);
    $ret['commands'] = array();
  }
return $ret;
}

exit (0)

/* vim: set expandtab tabstop=2 shiftwidth=2: */
?>
up
down
17monte at ispi dot net ¶22 years ago
I had a problem with the $argv values getting split up when they contained plus (+) signs. Be sure to use the CLI version, not CGI to get around it.

Monte
up
down
16linn at backendmedia dot com ¶21 years ago
For those of you who want the old CGI behaviour that changes to the actual directory of the script use:
chdir(dirname($_SERVER['argv'][0]));

at the beginning of your scripts.
up
down
17goalain eat gmail dont com ¶18 years ago
If your php script doesn't run with shebang (#!/usr/bin/php),

and it issues the beautifull and informative error message:

"Command not found."  just dos2unix yourscript.php

et voila.



If you still get the "Command not found." 

Just try to run it as ./myscript.php , with the "./"

if it works - it means your current directory is not in the executable search path.



If your php script doesn't run with shebang (#/usr/bin/php),

and it issues the beautifull and informative message:

"Invalid null command." it's probably because the "!" is missing in the the shebang line (like what's above) or something else in that area.



\Alon
up
down
16rob ¶18 years ago
i use emacs in c-mode for editing.  in 4.3, starting a cli script like so:

#!/usr/bin/php -q /* -*- c -*- */
<?php

told emacs to drop into c-mode automatically when i loaded the file for editing.  the '-q' flag didn't actually do anything (in the older cgi versions, it suppressed html output when the script was run) but it caused the commented mode line to be ignored by php.

in 5.2, '-q' has apparently been deprecated.  replace it with '--' to achieve the 4.3 invocation-with-emacs-mode-line behavior:

#!/usr/bin/php -- /* -*- c -*- */
<?php

don't go back to your 4.3 system and replace '-q' with '--'; it seems to cause php to hang waiting on STDIN...
up
down
9sam marshall ¶6 years ago
When using the -R flag, the name of the variable containing the content of the current line (not including the LF) is $argn.

For example you can do this code:

cat file.txt | php -R 'echo $argn . "\n";'

This will just output each line of the input file without doing anything to it.
ขึ้น
ลง
16Eric Dot Brison ที่ Anakeen Dot Com ¶18 ปีที่แล้ว
Just a variant of previous script to accept arguments with '=' also
<?php
function arguments($argv) {
    $_ARG = array();
    foreach ($argv as $arg) {
      if (ereg('--([^=]+)=(.*)',$arg,$reg)) {
        $_ARG[$reg[1]] = $reg[2];
      } elseif(ereg('-([a-zA-Z0-9])',$arg,$reg)) {
            $_ARG[$reg[1]] = 'true';
        }
   
    }
  return $_ARG;
}
?>
$ php myscript.php --user=nobody --password=secret -p --access="host=127.0.0.1 port=456" 
Array
(
    [user] => nobody
    [password] => secret
    [p] => true
    [access] => host=127.0.0.1 port=456
)
ขึ้น
ลง
12docey ¶19 ปีที่แล้ว
dunno if this is on linux the same but on windows evertime
you send somthing to the console screen php is waiting for
the console to return. therefor if you send a lot of small 
short amounts of text, the console is starting to be using 
more cpu-cycles then php and thus slowing the script.

take a look at this sheme:
cpu-cycle:1 ->php: print("a");
cpu-cycle:2 ->cmd: output("a");
cpu-cycle:3 ->php: print("b");
cpu-cycle:4 ->cmd: output("b");
cpu-cycle:5 ->php: print("c");
cpu-cycle:6 ->cmd: output("c"); 
cpu-cylce:7 ->php: print("d");
cpu-cycle:8 ->cmd: output("d"); 
cpu-cylce:9 ->php: print("e");
cpu-cycle:0 ->cmd: output("e"); 

on the screen just appears "abcde". but if you write 
your script this way it will be far more faster:
cpu-cycle:1 ->php: ob_start();
cpu-cycle:2 ->php: print("abc");
cpu-cycle:3 ->php: print("de");
cpu-cycle:4 ->php: $data = ob_get_contents();
cpu-cycle:5 ->php: ob_end_clean();
cpu-cycle:6 ->php: print($data);
cpu-cycle:7 ->cmd: output("abcde");

now this is just a small example but if you are writing an
app that is outputting a lot to the console, i.e. a text
based screen with frequent updates, then its much better 
to first cach all output, and output is as one big chunk of
text instead of one char a the time. 

ouput buffering is ideal for this. in my script i outputted
almost 4000chars of info and just by caching it first, it
speeded up by almost 400% and dropped cpu-usage.

because what is being displayed doesn't matter, be it 2
chars or 40.0000 chars, just the call to output takes a 
great deal of time. remeber that.

maybe someone can test if this is the same on unix-based
systems. it seems that the STDOUT stream just waits for 
the console to report ready, before continueing execution.
up
down
9bluej100@gmail ¶18 years ago
In 5.1.2 (and others, I assume), the -f form silently drops the first argument after the script name from $_SERVER['argv']. I'd suggest avoiding it unless you need it for a special case.
up
down
10obfuscated at emailaddress dot com ¶20 years ago
This posting is not a php-only problem, but hopefully will save someone a few hours of headaches.  Running on MacOS (although this could happen on any *nix I suppose), I was unable to get the script to execute without specifically envoking php from the command line:

[macg4:valencia/jobs] tim% test.php
./test.php: Command not found.

However, it worked just fine when php was envoked on the command line:

[macg4:valencia/jobs] tim% php test.php
Well, here we are...  Now what?

Was file access mode set for executable?  Yup.

[macg4:valencia/jobs] tim% ls -l 
total 16
-rwxr-xr-x  1 tim  staff   242 Feb 24 17:23 test.php

And you did, of course, remember to add the php command as the first line of your script, yeah?  Of course.

#!/usr/bin/php
<?php print "Well, here we are...  Now what?\n"; ?>

So why dudn't it work?  Well, like I said... on a Mac.... but I also occasionally edit the files on my Windows portable (i.e. when I'm travelling and don't have my trusty Mac available)...  Using, say, WordPad on Windows... and BBEdit on the Mac...

Aaahhh... in BBEdit check how the file is being saved!  Mac?  Unix?  or Dos?  Bingo.  It had been saved as Dos format.  Change it to Unix:

[macg4:valencia/jobs] tim% ./test.php
Well, here we are...  Now what?
[macg4:valencia/jobs] tim% 

NB: If you're editing your php files on multiple platforms (i.e. Windows and Linux), make sure you double check the files are saved in a Unix format...  those \r's and \n's 'll bite cha!
up
down
13Adam, php(at)getwebspace.com ¶22 years ago
Ok, I've had a heck of a time with PHP > 4.3.x and whether to use CLI vs CGI. The CGI version of 4.3.2 would return (in browser):
---
No input file specified.
---

And the CLI version would return:
---
500 Internal Server Error
---

It appears that in CGI mode, PHP looks at the environment variable PATH_TRANSLATED to determine the script to execute and ignores command line. That is why in the absensce of this environment variable, you get "No input file specified." However, in CLI mode the HTTP headers are not printed. I believe this is intended behavior for both situations but creates a problem when you have a CGI wrapper that sends environment variables but passes the actual script name on the command line.

By modifying my CGI wrapper to create this PATH_TRANSLATED environment variable, it solved my problem, and I was able to run the CGI build of 4.3.2
up
down
8Popeye at P-t-B dot com ¶22 years ago
In *nix systems, use the WHICH command to show the location of the php binary executable. This is the path to use as the first line in your php shell script file. (#!/path/to/php -q) And execute php from the command line with the -v switch to see what version you are running.

example:

# which php
/usr/local/bin/php
# php -v
PHP 4.3.1 (cli) (built: Mar 27 2003 14:41:51)
Copyright (c) 1997-2002 The PHP Group
Zend Engine v1.3.0, Copyright (c) 1998-2002 Zend Technologies

In the above example, you would use: #!/usr/local/bin/php

Also note that, if you do not have the current/default directory in your PATH (.), you will have to use ./scriptfilename to execute your script file from the command line (or you will receive a "command not found" error). Use the ENV command to show your PATH environment variable value.
ขึ้น
ลง
10stromdotcom ที่ hotmail dot com ¶19 ปีที่แล้ว
Spawning php-win.exe as a child process to handle scripting in Windows applications has a few quirks (all having to do with pipes between Windows apps and console apps).

To do this in C++:

// We will run php.exe as a child process after creating
// two pipes and attaching them to stdin and stdout
// of the child process
// Define sa struct such that child inherits our handles

SECURITY_ATTRIBUTES sa = { sizeof(SECURITY_ATTRIBUTES) };
sa.bInheritHandle = TRUE;
sa.lpSecurityDescriptor = NULL;

// Create the handles for our two pipes (two handles per pipe, one for each end)
// We will have one pipe for stdin, and one for stdout, each with a READ and WRITE end
HANDLE hStdoutRd, hStdoutWr, hStdinRd, hStdinWr;

// Now create the pipes, and make them inheritable
CreatePipe (&hStdoutRd, &hStdoutWr, &sa, 0))
SetHandleInformation(hStdoutRd, HANDLE_FLAG_INHERIT, 0);
CreatePipe (&hStdinRd, &hStdinWr, &sa, 0)
SetHandleInformation(hStdinWr, HANDLE_FLAG_INHERIT, 0);

// Now we have two pipes, we can create the process
// First, fill out the usage structs
STARTUPINFO si = { sizeof(STARTUPINFO) };
PROCESS_INFORMATION pi;
si.dwFlags = STARTF_USESTDHANDLES;
si.hStdOutput = hStdoutWr;
si.hStdInput  = hStdinRd;

// And finally, create the process
CreateProcess (NULL, "c:\\php\\php-win.exe", NULL, NULL, TRUE, NORMAL_PRIORITY_CLASS, NULL, NULL, &si, &pi);

// Close the handles we aren't using
CloseHandle(hStdoutWr);
CloseHandle(hStdinRd);

// Now that we have the process running, we can start pushing PHP at it
WriteFile(hStdinWr, "<?php echo 'test'; ?>", 9, &dwWritten, NULL);

// When we're done writing to stdin, we close that pipe
CloseHandle(hStdinWr);

// Reading from stdout is only slightly more complicated
int i;

std::string processed("");
char buf[128];

while ( (ReadFile(hStdoutRd, buf, 128, &dwRead, NULL) && (dwRead != 0)) ) {
    for (i = 0; i < dwRead; i++)
        processed += buf[i];
}    

// Done reading, so close this handle too
CloseHandle(hStdoutRd);

A full implementation (implemented as a C++ class) is available at http://www.stromcode.com
up
down
8losbrutos at free dot fr ¶17 years ago
an another "another variant" :

<?php
function arguments($argv)
{
  $_ARG = array();
  foreach ($argv as $arg)
  {
    if (preg_match('#^-{1,2}([a-zA-Z0-9]*)=?(.*)$#', $arg, $matches))
    {
      $key = $matches[1];
      switch ($matches[2])
      {
        case '':
        case 'true':
          $arg = true;
          break;
        case 'false':
          $arg = false;
          break;
        default:
          $arg = $matches[2];
      }
      $_ARG[$key] = $arg;
    }
    else
    {
      $_ARG['input'][] = $arg;
    }
  }
  return $_ARG;
}
?>

$php myscript.php arg1 -arg2=val2 --arg3=arg3 -arg4 --arg5 -arg6=false

Array
(
    [input] => Array
        (
            [0] => myscript.php
            [1] => arg1
        )

    [arg2] => val2
    [arg3] => arg3
    [arg4] => true
    [arg5] => true
    [arg5] => false
)
ขึ้น
ลง
8อเล็กซานเดอร์ พลาคิดิน ¶22 ปีที่แล้ว
How to change current directory in PHP script to script's directory when running it from command line using PHP 4.3.0?
(you'll probably need to add this to older scripts when running them under PHP 4.3.0 for backwards compatibility)

Here's what I am using:
chdir(preg_replace('/\\/[^\\/]+$/',"",$PHP_SELF));

Note: documentation says that "PHP_SELF" is not available in command-line PHP scripts. Though, it IS available. Probably this will be changed in future version, so don't rely on this line of code...

Use $_SERVER['PHP_SELF'] instead of just $PHP_SELF if you have register_globals=Off
ขึ้น
ลง
6pyxl ที่ jerrell dot com ¶23 ปีที่แล้ว
Assuming --prefix=/usr/local/php, it's better to create a symlink from /usr/bin/php or /usr/local/bin/php to target /usr/local/php/bin/php so that it's both in your path and automatically correct every time you rebuild.  If you forgot to do that copy of the binary after a rebuild, you can do all kinds of wild goose chasing when things break.
ขึ้น
ลง
5james_s2010 ที่ NOSPAM dot hotmail dot com ¶17 ปีที่แล้ว
I was looking for a way to interactively get a single character response from user. Using STDIN with fread, fgets and such will only work after pressing enter. So I came up with this instead:



#!/usr/bin/php -q

<?php

function inKey($vals) {

    $inKey = "";

    While(!in_array($inKey,$vals)) {

        $inKey = trim(`read -s -n1 valu;echo \$valu`);

    }

    return $inKey;

}

function echoAT($Row,$Col,$prompt="") {

    // Display prompt at specific screen coords

    echo "\033[".$Row.";".$Col."H".$prompt;

}

    // Display prompt at position 10,10

    echoAT(10,10,"Opt : ");



    // Define acceptable responses

    $options = array("1","2","3","4","X");



    // Get user response

    $key = inKey($options);



    // Display user response & exit

    echoAT(12,10,"Pressed : $key\n");

?>



Hope this helps someone.
+เพิ่มโน้ต
หน้าตา
การรับรองความถูกต้อง HTTP ด้วย PHP
คุกกี้
เซ สชัน
การจัดการการอัปโหลดไฟล์
การใช้ไฟล์ระยะไกล
การจัดการการเชื่อมต่อ
การเชื่อมต่อฐานข้อมูลถาวร
การใช้บรรทัดคําสั่ง
การเก็บขยะ
การติดตามแบบไดนามิก DTrace
ลิขสิทธิ์ © 2001-2025 กลุ่มเอกสาร PHP PHP.net ของฉัน ติดต่อ เว็บไซต์ PHP.net อื่นๆ นโยบายความเป็นส่วนตัว
