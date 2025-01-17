# Net Gearman

## About

Net_Gearman is a package for interfacing with Gearman. Gearman is a system to farm out work to other machines, dispatching function calls to machines that are better suited to do work, to do work in parallel, to load balance lots of function calls, or to call functions between languages.

## Installation

```
$ composer require brianlmoon/net_gearman
```

## Examples

### Client

```
$client = new Net_Gearman_Client("localhost");
$set = new Net_Gearman_Set();
$task = new Net_Gearman_Task("Reverse_String", "foobar");
$task->attachCallback(
    function($func, $handle, $result){
        print_r($result)
    }
);
$set->addTask($task);
$client->runSet($set, $timeout);
```

### Job

```
class Reverse_String extends Net_Gearman_Job_Common {

    public function run($workload) {
        $result = strrev($workload);
        return $result;
    }
}
```

### Worker

For easiest use, use GearmanManager for running workers. See: https://github.com/brianlmoon/GearmanManager

```
$worker = new Net_Gearman_Worker('localhost');
$worker->addAbility('Reverse_String');
$worker->beginWork();
```

## Development

### Running Tests

From the project root:

#### Install
1. composer install

#### Run the unit tests
1. vendor/bin/phpunit

#### Run the functional tests
1. Start up your gearman job server
2. Copy phpunit.xml.dist to phpunit.xml
3. Update the `NET_GEARMAN_TEST_SERVER` constant in `phpunit.xml` (if necessary)
4. vendor/bin/phpunit --group functional
