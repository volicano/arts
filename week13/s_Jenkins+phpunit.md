## ARTS

### Tip

## Jenkins+phpunit+laravel单元测试文档

 软件版本说明
PHP版本：大于等于7.0(目前使用版本7.0.26)，根据线上环境应用代码的部署环境；测试PHP5.6环境无法运行，PHP7.1已经PHP7.1以上版本没有测试过

Nginx：没有要求，建议与线上业务版本一致

PHPUnit：4.8.36 (Laravel框架composer而来)

PHP xdebug扩展：2.5.0(与php版本相对应即可)，用于phpunit代码覆盖率报告生成

软件安装说明
1、PHPUnit安装方式
1)、在Laravel框架上的composer.json文件中指定版本，然后composer update自动获取



2)、直接单独安装phpunit安装包

$ wget -O phpunit https://phar.phpunit.de/phpunit-4.phar

$ chmod +x phpunit

$ ./phpunit –version

2、PHP7.0.26 xdebug扩展安装
1)、下载xdebug软件安装包并解压

$ wget -P /usr/local/programs/src https://xdebug.org/files/xdebug-2.5.0.tgz

$ cd /usr/local/programs/src && tar -zxvf xdebug-2.5.0.tgz

2)、编译安装xdebug

$ cd /usr/local/programs/src/xdebug-2.5.0

$ /usr/local/programs/php/bin/phpize

$ ./configure --with-php-config=/usr/local/programs/php/bin/php-config

$ make && make install

如果安装正确，此时在/usr/local/programs/php/lib/php/extensions/no-debug-zts-20151012目录下生成一个xdebug.so的动态扩展模块文件

3、修改php配置文件，添加xdebug扩展模块
编辑/usr/local/programs/php/etc/php.ini，将以下文件添加到最后一行

zend_extension=”/usr/local/programs/php/lib/php/extensions/no-debug-zts-20151012/xdebug.so”

4、配置生效以及验证
重启nginx和php程序，运行php -m验证xdebug模块是否已经安装

php和nginx安装
忽略

phpunit单元测试使用说明
1、phpunit command
$ phpunit –help，参数不做详细解释，重要信息字面意思就能看懂

PHPUnit 4.8.36 by Sebastian Bergmann and contributors.

Usage: phpunit [options] UnitTest [UnitTest.php]

       phpunit [options] <directory>

Code Coverage Options:

  --coverage-clover <file>           Generate code coverage report in Clover XML format.

  --coverage-crap4j <file>           Generate code coverage report in Crap4J XML format.

  --coverage-html <dir>               Generate code coverage report in HTML format.

  --coverage-php <file>               Export PHP_CodeCoverage object to file.

  --coverage-text=<file>              Generate code coverage report in text format. Default: Standard output.

  --coverage-xml <dir>                Generate code coverage report in PHPUnit XML format.

 

Logging Options:

  --log-junit <file>                        Log test execution in JUnit XML format to file.

  --log-tap <file>                          Log test execution in TAP format to file.

  --log-json <file>                        Log test execution in JSON format.

  --testdox-html <file>                 Write agile documentation in HTML format to file.

  --testdox-text <file>                  Write agile documentation in Text format to file.

 

Test Selection Options:

  --filter <pattern>                       Filter which tests to run.

  --testsuite <name>                   Filter which testsuite to run.

  --group                                     Only runs tests from the specified group(s).

  --exclude-group ...                    Exclude tests from the specified group(s).

  --list-groups                              List available test groups.

  --test-suffix ...                           Only search for test in files with specified suffix(es). Default: Test.php,.phpt

 

Test Execution Options:

  --report-useless-tests              Be strict about tests that do not test anything.

  --strict-coverage                      Be strict about unintentionally covered code.

  --strict-global-state                  Be strict about changes to global state

  --disallow-test-output              Be strict about output during tests.

  --enforce-time-limit                  Enforce time limit based on test size.

  --disallow-todo-tests               Disallow @todo-annotated tests.

  --process-isolation                  Run each test in a separate PHP process.

  --no-globals-backup                Do not backup and restore $GLOBALS for each test.

  --static-backup                        Backup and restore static attributes for each test.

  --colors=<flag>                        Use colors in output ("never", "auto" or "always").

  --columns <n>                         Number of columns to use for progress output.

  --columns max                        Use maximum number of columns for progress output.

  --stderr                                    Write to STDERR instead of STDOUT.

  --stop-on-error                        Stop execution upon first error.

  --stop-on-failure                      Stop execution upon first error or failure.

  --stop-on-risky                        Stop execution upon first risky test.

  --stop-on-skipped                   Stop execution upon first skipped test.

  --stop-on-incomplete              Stop execution upon first incomplete test.

  -v|--verbose                            Output more verbose information.

  --debug                                   Display debugging information during test execution.

  --loader <loader>                   TestSuiteLoader implementation to use.

  --repeat <times>                     Runs the test(s) repeatedly.

  --tap                                        Report test execution progress in TAP format.

  --testdox                                 Report test execution progress in TestDox format.

  --printer <printer>                   TestListener implementation to use.

 

Configuration Options:

  --bootstrap <file>                    A "bootstrap" PHP file that is run before the tests.

  -c|--configuration <file>           Read configuration from XML file.

  --no-configuration                    Ignore default configuration file (phpunit.xml).

  --no-coverage                          Ignore code coverage configuration.

  --include-path <path(s)>         Prepend PHP's include_path with given path(s).

  -d key[=value]                         Sets a php.ini value.

 

Miscellaneous Options:

  -h|--help                                   Prints this usage information.

  --version                                  Prints the version and exits.

  --check-version                        Check whether PHPUnit is the latest version.

  --self-update                            Update PHPUnit to the latest version within the same release series.

  --self-upgrade                          Upgrade PHPUnit to the latest version.

2、phpunit配置文件phpunit.xml
phpunit单元测试的核心配置文件；phpunit.xml模板配置文件说明如下：

phpunit.xml文件重要配置段，分别为：

1)、phpunit：phpunit启动配置段

2)、testsuites：测试套件

3)、filter：代码覆盖白名单配置段

4)、logging：phpunit选项参数配置段

5)、php：php.ini以及常量和全局变量配置段

示例配置文件：

<?xml version="1.0" encoding="UTF-8"?>

<phpunit bootstrap="../../../bootstrap/autoload.php"           # 测试之前加载bootstrap/autoload.php文件，实现初始化，以及定义测试文件目录都可在这定义

         backupGlobals="false"                                                      # phpunit全局变量的备份参数，此处禁用

         backupStaticAttributes="false"                                          # 禁用备份静态属性

         verbose="true"                                                                  # 显示phpunit执行详细信息

         colors="false"                                                                    # 禁用颜色显示

         convertErrorsToExceptions="true"                                    # 默认情况下，phpunit将安装一个错误处理程序，将以下错误转换为异常：E_WARNING、E_NOTICE、E_USER_ERROR、E_USER_WARNING、E_USER_NOTICE、E_STRICT、

                                                                                                   #   E_RECOVERABLE_ERROR、E_DEPRECATED、E_USER_DEPRECATED；

         convertNoticesToExceptions="true"                                  # 对应上面的E_NOTICE、E_USER_NOTICE、E_STRICT错误

         convertWarningsToExceptions="true"                               # 对应上面的E_WARNING、E_USER_WARNING

         processIsolation="false"                                                    # 禁用phpunit进程隔离

         stopOnFailure="false"                                                       # 禁用失败停止运行phpunit

         syntaxCheck="true">                                                        # 开启phpunit语法检查

  <testsuites>                                                                             # 测试套件配置段

    <testsuite name="small">                                                      # 单元模块测试名字

      <directory suffix="Test.php">tests/Framework</directory>  # 将递归遍历tests/Framework目录中所有在*Test.php文件，并逐一对脚本进行单元测试

      <file>tests/Framework/HomeTest.php</file>                  # 同样可以添加单一的测试文件

<!--  <directory suffix="Test.php">tests/Extensions</directory> # 同上

      <directory suffix="Test.php">tests/Runner</directory>        # 同上

      <directory suffix="Test.php">tests/Util</directory>               # 同上

-->

      # 备注：文件测试顺序按照书写前后顺序依次执行

    </testsuite>

    <testsuite name="large">                                                      # 大的单元测试用力配置，说明同上

      <directory suffix=".phpt">tests/TextUI</directory>

      <directory suffix=".phpt">tests/Regression</directory>

    </testsuite>

  </testsuites>

  <filter>                                                                                     # 代码覆盖报告率白名单设置

<whitelist processUncoveredFilesFromWhitelist="true">          # 将白名单中包含的所有文件全部加入到代码覆盖率报告中

      <directory suffix=".php">./app</directory>

      <exclude>

       <file>./app/Modules/Api/Http/routes.php</file>                 # 指定文件

       <directory><directory>                                                   # 同样也可指定目录，为顶层目录的子目录

      </exclude>

    </whitelist>

  </filter>

  <logging>                                                                                 # 配置测试执行期间的日志记录，如果此项配置，则无需在脚本运行时指定输出格式参数

    <log type="coverage-html" target="/tmp/report" lowUpperBound="35" highLowerBound="70"/>

    <log type="coverage-clover" target="/tmp/coverage.xml"/>

    <log type="coverage-php" target="/tmp/coverage.serialized"/>

    <log type="coverage-text" target="php://stdout" showUncoveredFiles="false"/>

    <log type="junit" target="/tmp/logfile.xml" logIncompleteSkipped="false"/>

    <log type="testdox-html" target="/tmp/testdox.html"/>

    <log type="testdox-text" target="/tmp/testdox.txt"/>

</logging>

  <php>                                                                                      # php全局变量配置段

    <const name="PHPUNIT_TESTSUITE" value="true"/>

  </php>

</phpunit>

jenkins集成phpunit运行单元测试