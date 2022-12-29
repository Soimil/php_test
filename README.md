1. Autowiring in dependency injection allows us to use, among others, variables that are defined in service.yaml in the path:
    ```yaml
    services:
        _default:
            bind:
    ```
    and all services that are public or have the `autowire: true` parameter set. Autowiring reads the type data in the controller or action methods in the Controller classes, then autowiring using the settings from the config folder if necessary and passing services (the most common service used in Controllers is `Symfony\Component\HttpFoundation\Request`).

2. When sending a request, the `index.php` file is called which creates the `Request` object using global variables `($_GET, $_POST, $_COOKIE, $_FILES, $_SERVER)`, initializes the Kernel object to consume the `Request` object and return `Response` object.
During `Kernel::handle` function the `handleRaw` method is called which includes:
    * Adding `Request` to `RequestStack` and calling `RequestEvent` which is listened to by listeners one of them is `RouterListener` which checks URL to match appropriate `Controller`.
    * After matching the `Controller`, a `ControllerEvent` is called by Symfony which is caught by, among others, `ControllerListener` where it is checked what method is to be called and `ParamConverterListener` to check and convert controller action arguments.
    * The action in the controller is called, which returns the `Response` object and the `ResponseEvent` and `FinishRequestEvent` sent, and then the `Request` is removed from the `RequestStack`.
3. In a few words:
    * Thanks to `traits`, I can quickly use arguments and parameters in more than one class, a good example are Doctrine extensions like timestampable (use TimestampableEntity;).
    * Every function that returns `yield` is a `generator function`. The `yield` statement is very similar to the `return` statement, except that instead of stopping the function from executing and returning, `yield` delivers a value to the code passing through the generator and stops the generator function from executing.
    * Basic difference:
    `?` will check if the defined value is true or false and if the given value does not exist return false and warning. `??` checks if a given value is defined.

    | Expression | echo ($x ?: 'hello') | echo ($x ?? 'hello') |
    |---|---|---|
    | $x = ""; | 'hello' | "" |
    | $x = null; | 'hello' | 'hello' |
    | $x; | 'hello' (and Notice: Undefined variable: x) | 'hello' |
    | $x = []; | 'hello' | [] |
    | $x = ['a', 'b']; | ['a', 'b'] | ['a', 'b'] |
    | $x = false | 'hello' | false |
    | $x = true; | true | true |
    | $x = 1; | 1 | 1
    | $x = 0; | 'helo' | 0 |
    | $x = -1 | -1 | -1 |
    | $x = '1' | '1' | '1' |
    | $x = '0'; | 'helo' | '0' |
    | $x = 'random'; | 'random' | 'random' |
    | $x = new stdClass; | object(stdClass) | object(stdClass) |

    * `<=>` Return `0` if values on either side are `equal`. Return `1` if the value on the `left is greater`, Return `-1` if the value on the `right is greater`.
4. Example
    ```
    /((\+44(\s0\s|\s)?)|0)7\d{3}(\s)?\d{6}/m
    ```
5. I don't know the answer to this question and I'm not sure I understand it.
6. Example
    ```PHP
    foreach (range(1, 100) as $number) {
        $result = '';
        if (($number % 3) === 0) {
            $result .= 'fizz';
        }
        if (($number % 5) === 0) {
            $result .= 'buzz';
        }
        if (strlen($result) === 0) {
            continue;
        }
        echo $result."\n";
    }
    ```
    Short version
    ```PHP
    foreach (range(1, 100) as $number) {
        $result = '';
        if (($number % 3) === 0) $result .= 'fizz';
        if (($number % 5) === 0) $result .= 'buzz';
        if (strlen($result) === 0) continue;
        echo $result."\n";
    }
    ```
7. Example:
    ```PHP
    function sum_valies(int $start, int $end) {
        if ($start <= 0 || $end <= 0) {
            return 1;
        }
        if ($start > $end) {
            return 2;
        }

        $array = [10, 20, 30, 40, 50, 60, 70, 80, 90, 100];
        $start_index = array_search($start, $array);
        $end_index = array_search($end, $array);
        if (!$start_index) {
            return 3;
        }

        if ($end_index) {
            $array = array_slice($array, $start_index, ($end_index + 1) - count($array));
        } else {
            $array = array_slice($array, $start_index);
        }

        return array_sum($array);
    }
    ```
8. Software operating as a non-relational database storing data in a key-value structure in the server's operational memory. It can be used as an application cache or for queuing (messenger).

    Advantages:
    1. Super fast
    2. Simple and easy to use
    3. It supports almost all data structures

    Disadvantages:
    1. Redis dictates client configuration (client must know cluster topology)
    2. Uses large amounts of memory
9.  Example:
    ```PHP
    $array = ['200','12','69','3','250','50','500'];
    rsort($array);
    echo array_values($array)[1]."\n";
    ```

10. Examples:
    * A
    ```Postgresql
    SELECT d.name, SUM(e.salary) as sum_salary FROM department d JOIN employee e on d.id = e.department_id GROUP BY d.name;
    ```
    * B - where a given Manager id = 1
    ```Postgresql
    SELECT avg(e.salary) FROM department d LEFT JOIN employee e on d.id = e.department_id WHERE d.manager_id = 1;
    ```
    * C - where a given Department id = 1
    ```Postgresql
    SELECT sum(em.salary) / sum(e.salary) * 100 as testing FROM employee e LEFT JOIN employee em on e.id = em.id and em.department_id = 1;
    ```

    * D

    If we remove a manager, the department dependent on him will also be deleted, as well as all relations between the deleted department and employees (department_id will be set to null).
