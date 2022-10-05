# Translations

- ![ru](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Russia.png) **[Russian](https://github.com/roistat/php-code-conventions)**

# Table of Contents
  1. [Introduction](#Introduction)
  2. [Goals](#Goals)
  3. [Principles](#Principles)
  4. [General rules](#General-rules)
  5. [Rules of business-logic splitting](#Rules-splitting-business-logic)
  6. [Files](#Files)
  7. [Variables](#Variables)
  8. [Variables and Methods](#variables-and-methods)
  9. [Arrays](#Arrays)
  10. [Strings](#Strings)
  11. [Dates](#Dates)
  12. [Namespaces](#Namespaces)
  13. [Functions](#Functions)
  14. [Return results from methods](#Return-results-methods)
  15. [Classes](#Classes)
  16. [Objects](#objects)
  17. [Comments](#Comments)
  18. [Exceptions](#Exceptions)
  19. [Cloud](#Cloud)
  20. [Pull Requests (PR)](#pull-requests-pr)
  21. [Templates](#Templates)
  22. [-](#-)
  23. [Conditions](#Conditions)
  24. [Ternary Operators](#Ternary-operators)
  25. [Tests](#Tests)
  26. [Chaining Objects](#Chaining)
  27. [Cron Jobs and Scripts](#Cron)
  28. [Authors](#Authors)
  
## **Introduction**

This document contains code writing rules (Code Conventions) in the company Roistat. We have got a lot of experience in developing complex projects, with which we decided to share with others. You can take this document as is or use it as the basis for your own Code Conv.

Work on English translation is in progress. You can help us a lot if you make pull request.
Here you can always find an up to date version of our Code Conv, as we are regularly referring to it in our Code Review.

О нашем опыте использования Code Conv вы можете прочитать в [статье на Хабре](https://habrahabr.ru/company/roistat/blog/352762/).

Code Conv — is a set of rules to follow when writing any kind of code. We treat Code Style and Code Conv separately. For us Code Style is purely visual side of code, e.g. placement of tabs, comas, parentheses, and others. Code Conv on the other hand is meaningful Summary of code, e.g. correct sequences of actions, fitting and meaningful variable and method names, correct overall code composition. Code Style is pretty easy to evaluate automatically, but checking Code Conv, in most cases, is a task only human can do.

Обратите внимание: Code Style в примерах может отличаться от Code Style вашего проекта. Придерживаться надо тому Code Style, который принят у вас. Code Conv не об этом. 

## **Goals**
The main goal of Code Conv — сохранение низкой стоимости разработки и поддержки кода на длинной дистанции.

Основные ценности, помогающие достичь этой цели:

**Readability**

Код должен легко читаться, а не легко записываться. Это значит, что такие вещи как синтаксический сахар (если он направлен на ускорение записи, а не дальнейшего чтения кода) вредны.
Обратите внимание, что быстродействие кода не является ценностью, поэтому не самый оптимальный цикл, но удобный для понимания, будет лучше, чем быстрый, но сложный. Не нужно экономить переменные, буквы для их названий, оперативную память и так далее.

**Vandalproof**

Код надо писать так, чтобы у разработчика, который с ним будет работать, было как можно меньше возможности внести ошибку. Например, покрывайте тестами не только краевые условия, но и кейсы, которые могут появиться в результате доработок кода и рефакторинга.

**Maintaining the lowest entropy**

Энтропия — это количество информации, из которой состоит проект (информационная емкость проекта). Код проекта должен выполнять продуктовые требования с сохранением наименьшей возможной энтропии. 

## **Principles**

Принципы — это способы соблюдения описанных выше ценностей. Они чуть более детальны, содержат основные методологии разработки и подходы, которыми мы руководствуемся. 

Код должен быть:

- Понятным, явным. Явное лучше, чем неявное. Например, не должны использоваться магические методы. Также нельзя использовать `exit` и любые другие операторы, которые могут завершить или изменить работу процесса. 
- Удобным для использования сейчас
- Удобным для использования в будущем
- Должен стремиться к соблюдению принципов [KISS](https://ru.wikipedia.org/wiki/KISS_(%D0%BF%D1%80%D0%B8%D0%BD%D1%86%D0%B8%D0%BF)), [SOLID](https://ru.wikipedia.org/wiki/SOLID_(%D0%BE%D0%B1%D1%8A%D0%B5%D0%BA%D1%82%D0%BD%D0%BE-%D0%BE%D1%80%D0%B8%D0%B5%D0%BD%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%BD%D0%BE%D0%B5_%D0%BF%D1%80%D0%BE%D0%B3%D1%80%D0%B0%D0%BC%D0%BC%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5)), [DRY](https://ru.wikipedia.org/wiki/Don%E2%80%99t_repeat_yourself), [GRASP](https://ru.wikipedia.org/wiki/GRASP)
- Код должен обладать слабым зацеплением и высокой связностью (подробно это описано в GRASP). Любая часть системы должна иметь изолированную логику и при надобности внешний интерфейс, который позволяет с этой логикой работать. Любая внутренняя часть должна иметь возможность быть измененной без какого-либо ущерба внешним системам
- Код должен быть таким, чтобы его можно было автоматически отрефакторить в IDE (например, Find usages и Rename в PHPStorm). То есть должен быть слинкован типизацией и PHPDoc'ами
- В БД не должны храниться части кода (даже названия классов, переменных и констант), так как это делает невозможным автоматический рефакторинг
- Последовательным. Код должен читаться сверху вниз. Читающий не должен держать что-то в уме, возвращаться назад и интерпретировать код иначе. Например, надо избегать обратных циклов `do {} while ();` 
- Должен иметь минимальную [цикломатическую сложность](https://ru.wikipedia.org/wiki/%D0%A6%D0%B8%D0%BA%D0%BB%D0%BE%D0%BC%D0%B0%D1%82%D0%B8%D1%87%D0%B5%D1%81%D0%BA%D0%B0%D1%8F_%D1%81%D0%BB%D0%BE%D0%B6%D0%BD%D0%BE%D1%81%D1%82%D1%8C)

## **General Rules**

### 📖 Unused code is forbidden
Если код можно убрать, и работа системы от этого не изменится, его быть не должно.

Bad:
```php
if (false) {
    legacyMethodCall();
}
// ...
$legacyCondition = true;
if ($legacyCondition) {
    finalizeData($data);
}
```

Good:
```php
// ...
finalizeData($data);
```

### 📖 Functions of specific PHP version should not be used, if they can be avoided
Это упростит миграцию кода на новую версию языка. Часто в новой версии языка удаляются какие-либо функции или изменяется их работа. Чем меньше идет завязки на язык и его версию, тем лучше.

Специфичные функции всегда лучше использовать через функции-обёртки внутри проекта. Тогда в случае миграции придется исправлять одно место, а не тысячу.

Как понять, можно ли использовать встроенную в PHP функцию или нет?

- Если эта функция уже используется повсеместно в проекте, значит, её можете использовать и вы. Например, это может быть `explode`/`implode`. Если эти функции будут изменены в новой версии PHP, то в любом случае придется переделать много кода и делать это будет автоматика.

- Если эта функция не используется или используется только через обёртку в специализированном сервисе, то и вы использовать её можете только через обёртку (добавляется при необходимости).

Bad:
```php
if (ctype_digit($number)) {
    // ...
}
$distance = levenshtein($text1, $text2);
$urlParts = parse_url($url);
```

Good:
```php
if ($number >= 0) {
    // ...
}
$distance = $textService->levenshtein($text1, $text2);
$urlParts = $urlService->parseUrl($url);
```

### 📖 Instead of the missing scalar value use null
0 и пустую строку нельзя использовать в качестве показателя отсутствия значения.
```php
/**
 * @param string $title
 * @param string $message
 * @param string $date
 */
function sendEmail($title, $message = null, $date = null) {
    // ...
}

// сообщение не было передано
$object->sendEmail('Title', null, '2017-01-01');

// было передано пустое сообщение
$object->sendEmail('Title', '', '2017-01-01');
```

Однако, это правило не относится к массивам.

Bad:
```php
function deleteUsersByIds(array $ids = [], $someOption = false) {
    // ...
}

deleteUsersByIds(null, true);
```

Good:
```php
deleteUsersByIds([], true);
```

Итого: использование пустой строки почти всегда является ошибкой.

**[⬆ up](#Summary)**

## **Rules of business-logic splitting**

### 📖 Сервисы
Сервис – это класс без состояния, содержащий бизнес-логику. Данные для обработки сервис получает либо в виде параметров публичных методов, либо других сервисов. 

Сервис не может использовать в качестве источника данных глобальные переменные или окружение:

Bad:
```php
class User {

    public function loadUsers() {
        $path = getenv('DATA_PATH'); // так нельзя!
        // ...
    }
}
```

Good:
```php
class Env {
    
    /**
     * @return string
     */
    public function getDataPath(): string {
        return getenv('DATA_PATH');
    }
}

class User {

    /**
     * @var Env
     */
    private $_env;
    
    /**
     * @param Env $env
     */
    public function __construct(Env $env) {
        $this->_env = $env;
    }

    public function loadUsers() {
        $path = $this->_env->getDataPath();
        // ...
    }
}
```
Однако, это правило не работает, если получение данных из внешних источников — единственная бизнес-логика сервиса.

Для работы с хранилищем мы используем репозиторий. Это частный случай сервиса, но он получает данные из БД через адаптеры.

### 📖 Контроллеры
Контроллер принимает и обрабатывает запросы. Он получает параметры на вход, запрашивает данные из сервисов и возвращает представление.

### 📖 Модели
Модель — простой объект со свойствами, не содержащий никакой другой бизнес-логики, кроме геттеров и сеттеров. Геттер — метод, позволяющий получить какую-то информацию из внутреннего состояния объекта. Это не обязательно поле, как оно есть. Он может брать значения нескольких полей и делать простые манипуляции с ними (не запрашивая внешней продуктовой бизнес-логики). Сеттер — аналогично может изменять внутреннее состояние одного или нескольких полей без запросов «наружу».

Условный упрощенный пример:

```php
class Bill {
    public $id;
    public $sum;
    public $isPaid;
    public $paidDate
    // ...
    
    public function markAsPaid(\DateTime $paymentDate) {
        $this->isPaid = true;
        $this->paidDate = $paymentDate;
    }
}
```

Желательно делать модели неизменяемыми, см. [Работа с объектами](#Работа-с-объектами). Хотите больше гибкости — можно использовать [chain-объекты](#Использование-chain-объектов).

### 📖 Представления
Представлением в зависимости от требуемого ответа сервера может быть HTML-шаблон, API-объект или что-то иное. Обратите внимание, API-объект и модель данных это разные сущности, даже если у них совпадает название и все поля. Нельзя просто вернуть в JSON-ответе сервера модель из хранилища:

Bad:
```php
public function actionUsers(): Response {
    $users = $repository->loadUsers();
    return new Response(['data' => $users]);
}
```

Свойства модели хранилища могут поменяться из-за новых технических требований, но объект API это продукт, вы должны изменять его явно:

Good:
```php
// src/Entity/Api/User.php
namespace Entity\Api;

class User {
    public $id;
    public $name;
}

// src/Controller/Api/User.php
public function actionUsers(): Response {
    $users = $repository->loadUsers();
    $apiUsers = array_map(function ($user) {
        return $this->_convertUserToApiObject($user);
    }, $users);
    return new Response(['data' => $apiUsers);
}

private function _convertUserToApiObject(Entity\Mapper\User $user): Entity\Api\User {
    // ...
}
```

**[⬆ up](#Summary)**

## **Files**

### 📖 Названия файлов пишутся строчными буквами через underscore
Кроме случаев, когда внутри файла содержится класс. В таком случае файл должен повторять названия класса, то есть должен быть написан в стиле UpperCamelCase. Аналогично обычные директории и пространства имён.

### 📖 Файлы классов должны быть расположены в директориях в соответствии со стандартом PSR-0

**[⬆ up](#Summary)**

## **Variables**

### 📖 Название переменных пишутся через camelCase

### 📖 Название переменных должно соответствовать содержанию
Нельзя писать короткие названия, например `$c`. Нельзя назвать переменную `$day` и хранить в ней массив статистических данных за этот день.

### 📖 Часто упоминаемые объекты именуются одинаково во всем проекте

Bad:
```php
$customer = new User();
$client = new User();
$object = new User();
```

Good:
```php
$user = new User();
```

### 📖 Признак объекта добавляется к названию
Если это отфильтрованный по какому-то признаку проект, то признак добавляется к названию. Например, `$unpaidProject`.

### 📖 Переменные, отражающие свойства объекта, должны включать название объекта

Bad:
```php
$project = new Project();
$name = $project->name;
$project = $project->name;
```

Good:
```php
$project = new Project();
$projectName = $project->name;
```

### 📖 Переменные по возможности должны называться на корректном английском

Bad:
```php
$usersStored = [];
```

Good:
```php
$storedUsers = [];
```

Исключение: сгруппированные по некому признаку поля или константы. В этом случае можно использовать префикс.

```php
class ProjectInfo {
    const STATUS_READY = 1;
    const STATUS_BLOCKED = 2;

    public $billingIsPaid;
    public $billingPaidDate;
    public $billingSum;
}
```

### 📖 К переменной нельзя обращаться по ссылке (через &)
Амперсанды могут использоваться только как логические или битовые операторы.

Bad:
```php
/**
 * @param string &$name
 */
function removePrefix(&$name) {
    // ...
}
```

Good:
```php
/**
 * @param string $name
 * @return string
 */
function removePrefix($name) {
    // ...
    return $result;
}
```

### 📖 Переменные и свойства объекта должны являться существительными и называться так, чтобы они правильно читались при использовании, а не при инициализации.

Bad:
```php
$object->expire_at
$object->setExpireAt($date);
$object->getExpireAt();
```

Good:
```php
$object->expiration_date;
$object->setExpirationDate($date);
$object->getExpirationDate();
```

### 📖 В названии переменной не должно быть указания типа
Нельзя писать `$projectsArray`, надо писать просто `$projects`. Это же касается и форматов (JSON, XML и т.п.), и любой другой не относящейся к предметной области информации.

Bad:
```php
$projectsList = $repository->loadProjects();
$projectsListIds = $utils->extractField('id', $projectsList);
```

Good:
```php
$projects = $repository->loadProjects();
$projectsIds = $utils->extractField('id', $projects);
```

### 📖 Нельзя изменять переменные, которые передаются в метод на вход 
Исключение — если эта переменная объект.

Bad:
```php
function parseText($text) {
    $text = trim($text);
    // ...
}
```

Good:
```php
function parseText($text) {
    $trimmedText = trim($text);
    // ...
}
```

### 📖 Каждая переменная должна быть объявлена на новой строке

Bad:
```php
$foo = false; $bar = true;
```

Good:
```php
$foo = false;
$bar = true;
```

### 📖 Нельзя нескольким переменным присваивать одно и то же значение

Bad:
```php
function loadUsers(array $ids) {
    $usersIds = $ids;
    // ...
}
```

### 📖 Оператор `clone` должен использоваться только в тех случаях, когда без него не обойтись
Также можно использовать `clone`, если без него код серьёзно усложнится, а с ним станет понятным и очевидным. Простой пример — клонирование объектов DateTime. Или использование клонирования для сравнения двух версий объекта: старой и новой. 

Bad:
```php
function loadAnalyticsData(\DateTime $intervalStart) {
    $intervalEnd = new \DateTime($intervalStart->format('Y-m-d H:i:s'));
    $intervalEnd->modify('+1 day');
}

function updateUser(User $user) {
    $oldUser = new User();
    $oldUser->id = $user->id;
    $oldUser->name = $user->name;
    // ...
    logObjectDiff($user, $oldUser);
}
```

Good:
```php
function loadAnalyticsData(\DateTime $intervalStart) {
    $intervalEnd = clone $intervalStart;
    $intervalEnd->modify('+1 day');
}

function updateUser(User $user) {
    $oldUser = clone $user;
    // ...
    logObjectDiff($user, $oldUser);
}
```

### 📖 Запрещено использовать результат операции присваивания

Bad:
```php
$foo = $bar = strlen($someVar);
```

Good:
```php
$bar = strlen($someVar);
$foo = $bar;
```


Bad:
```php
$this->_callSomeFunc($bar = strlen($foo));
```

Good:
```php
$bar = strlen($foo);
$this->_callSomeFunc($bar);
```


Bad:
```php
if (strlen($foo = json_encode($bar)) > 100) {
    // ...
}
```

Good:
```php
$foo = json_encode($bar);
if (strlen($foo) > 100) {
    // ...
}
```

### 📖 Нельзя использовать константы через метод `constant`

Bad:
```php
/**
 * @return string
 */
public function getProjectDir(): string {
    $prefix = 'ACME_';
    $name = $prefix . 'PROJECT_DIR';
    return constant($name);
}

/**
 * @return string
 */
public function getProjectDir(): string {
    return constant('ACME_PROJECT_DIR');
}
```

Good:
```php
/**
 * @return string
 */
public function getProjectDir(): string {
    return ACME_PROJECT_DIR;
}
```

**[⬆ up](#Summary)**

## **Variables and Methods**

### 📖 Названия boolean методов и переменных должны содержать глагол `is`, `has` или `can`

Переменные правильно называть, описывая их содержимое, а методы — задавая вопрос. Если переменная содержит свойство объекта, следуем правилу [признак объекта добавляется к названию](#-Признак-объекта-добавляется-к-названию).

Bad:
```php
$isUserValid = $user->valid();
$isProjectAnalytics = $accessManager->getProjectAccess($project, 'analytics');
```

Good:
```php
$userIsValid = $user->isValid();
$projectCanAccessAnalytics = $accessManager->canProjectAccess($project, 'analytics');
```

Геттеры именуются аналогично переменным:

```php
class User {
    private $_billingIsPaid;
    private $_isEnabled;

    public function isEnabled() {
        return $this->_isEnabled;
    }

    public function billingIsPaid() {
        return $this->_billingIsPaid;
    }
}
```

Такое именование позволяет легче читать условия:

```php
// if user is valid, then do something
if ($userIsValid) {
    // do something
}
```

### 📖 Запрещены отрицательные логические названия

Bad:
```php
if ($project->isInvalid()) {
    // ...
}
if ($project->isNotValid()) {
    // ...
}
if ($accessManager->isAccessDenied()) {
    // ...
}
```

Good:
```php
if (!$project->isValid()) {
    // ...
}
if (!$accessManager->isAccessAllowed()) {
    // ...
}
if ($accessManager->canAccess()) {
    // ...
}
```

### 📖 Не используйте boolean переменные (флаги) как параметры функции
Флаг в качестве параметра это признак того, что функция делает больше одной вещи, нарушая [принцип единственной ответственности (Single Responsibility Principle или SRP)](https://ru.wikipedia.org/wiki/%D0%9F%D1%80%D0%B8%D0%BD%D1%86%D0%B8%D0%BF_%D0%B5%D0%B4%D0%B8%D0%BD%D1%81%D1%82%D0%B2%D0%B5%D0%BD%D0%BD%D0%BE%D0%B9_%D0%BE%D1%82%D0%B2%D0%B5%D1%82%D1%81%D1%82%D0%B2%D0%B5%D0%BD%D0%BD%D0%BE%D1%81%D1%82%D0%B8). Избавляйтесь от них, выделяя код внтури логических блоков в отдельные ветви выполнения.

Bad:
```php
function someMethod() {
    // ...
    $projectNotificationIsEnabled = $notificationManager->isProjectNotificationEnabled($project);
    storeUser($user, $projectNotificationIsEnabled);
}

function storeUser(User $user, $isNotificationEnabled) {
    // ...
    if ($isNotificationEnabled) {
        notify('new user');
    }
}
```

Good:
```php
function someMethod() {
    // ...
    storeUser($user);
    if ($notificationManager->isProjectNotificationEnabled($project)) {
        notify('new user');
    }
}

function storeUser(User $user) {
    // ...
}
```

**[⬆ up](#Summary)**

## **Arrays**

### 📖 Для конкатенации массивов запрещено использовать оператор `+`.
Обратите внимание, что `array_merge` все числовые ключи приводит к `int`, даже если они записаны строкой.

Bad:
```php
return $initialData + $loadedData;
```

Good:
```php
namespace Service;

class ArrayUtils {

    /**
     * @param array $array1
     * @param array $array2
     * @return array
     */
    public function mergeArrays(array $array1, array $array2): array {
        return array_merge($array1, $array2);
    }
}

public function someMethod() {
    return $this->_arrayUtils->mergeArrays($initialData, $loadedData);
}
```

### 📖 Для проверки наличия ключа в ассоциативном массиве используем `array_key_exists`, а не `isset`
`isset` проверяет не ключ на его наличие, а значение этого ключа, если он есть. Это разные методы с разным поведением и назначением. Если вы хотите проверить значение ключа, то делайте это явно. Сначала явно проверьте наличие ключа через `array_key_exists` и обработайте ситуацию его отсутствия, затем приступайте к работе со значением.

Bad:
```php
function getProjectKey(array $requestData) {
    return isset($requestData['project_key']) ? $requestData['project_key'] : null;
}
```

Good:
```php
function getProjectKey(array $requestData) {
    if (!array_key_exists('project_key', $requestData)) {
        return null;
    }
    return $requestData['project_key'];
}
```

### 📖 Ассоциативный массив мы используем как hashmap
То есть не применяем разные встроенные в PHP инструменты. Приведем несколько очевидных примеров (однако, правило ими не исчерпывается):

#### 📖 Нельзя сортировать ассоциативные массивы

Bad:
```php
$arr = [
    'project_key' => 'foo',
    'key' => 'bar',
    'user_id' => 300,
];

uasort($arr);
```

#### 📖 Нельзя смешивать в массиве строковые и числовые ключи

Bad:
```php
$arr = [
    'project_key' => 'foo',
    'key' => 'bar',
    'user_id' => 300,
    1 => 'value1',
    2 => 'value2',
];

$arr[3] = 'value3';
```

#### 📖 Для проверки наличия значения по индексу в обычных (не ассоциативных) массивах используем `count($array) > N`

Bad:
```php
if (array_key_exists(1, $users)) {
    // ...
}
if (isset($users[1])) {
    // ...
}
```

Good:
```php
if (count($users) > 1) {
   // ... 
}
```

**[⬆ up](#Summary)**

## **Strings**

### 📖 Строки обрамляются одинарными кавычками
Двойные кавычки используются только, если:
* Внутри строки должны быть одинарные кавычки
* Внутри строки используется подстановка переменных
* Внутри строки используются спец. символы `\n`, `\r`, `\t` и т.д.

Bad:
```php
$string = "Some string";
$string = 'Some \'string\'';
$string = "\t".'Some string'."\n";
```

Good:
```php
$string = 'Some string';
$string = "Some 'string'";
$string = "\tSome string\n";
```

### 📖 Instead of unnecessary concatenation, we use variable substitution in double quotes using curly braces

Bad:
```php
$string = 'Object with type "' . $object->type() . '" has been removed';
```

Good:
```php
$string = "Object with type \"{$object->type()}\" has been removed";
```

**[⬆ up](#Summary)**

## **Dates**

### 📖 Дата всегда должна быть представлена DateTime, интервал как DateInterval

Bad:
```php
$date = $request->get('date');
$interval = 86400*30;
loadSomeData($date, $interval);
```

Good:
```php
$date = $this->_dateService->instance($request->get('date'));
$interval = new \DateInterval('P30D');
loadSomeData($date, $interval);
```

### 📖 Запрещено создавать объект даты при помощи `new \DateTime()`

В проекте для этого должен быть фабричный метод в сервисе для работы с датами.

Bad:
```php
$date = new \DateTime();
```

Good:
```php
$date = $this->_dateService->instance();
```

### 📖 Если дата должна быть представлена скалярным значением, необходимо использовать строку

* строка с датой и временем должна быть везде в одинаковом формате
* формат не должен включать временную зону, если для этого не особых требований
* при прочих равных в дате без часового пояса всегда подразумевается UTC0
* если строку по какой-то причине невозможно использовать, используем `int`

Bad:
```php
class User {
    public $creation_time;
}

$user->creation_time = time();
```

Good:
```php
class User {
    /**
     * @type string
     */
    public $creation_date;
}

$user->creation_date = '2018-01-18 12:54:11';
```

### 📖 При работе с интервалами/периодами запрещено указывать месяц или год

В зависимости от текущей даты месяц и год могут принимать разные временные промежутки (високосный и обычный год, разное количество дней в месяце). Вместо этого в качестве указания интервала используем дни, часы, минуты, секунды.

Bad:
```php
$dateTime = new \DateTime('-2 month');
$dateInterval = new \DateInterval('P2M');
```

Good:
```php
$dateTime = new $this->_dateTime->instance('-60 days');
$dateInterval = new \DateInterval('P60D');
```

Месяц или год необходимо использовать, если это напрямую указано в требованиях задачи как календарный месяц или календарный год.

**[⬆ up](#Summary)**

## **Namespaces**

### 📖 Все пространства имён должны быть подключены через `use` в начале файла. В самом коде не должно быть обратного слеша перед названием пространства имён

Bad:
```php
$object = new \Some\Object();
```

Good:
```php
use Some;
$object = new Some\Object();
```

### 📖 В свою очередь обычные классы без пространства имён не должны быть подключены через `use`

Bad:
```php
use TimeZone;
$date = new TimeZone('Europe\Moscow');
```

Good:
```php
$date = new \TimeZone('Europe\Moscow');
```

### 📖 Нельзя подключать несколько классов из одного пространства имён через `use`

Bad:
```php
use Entity\User;
use Entity\Project;
 
$user = new User();
$project = new Project();
```

Good:
```php
use Entity;
  
$user = new Entity\User();
$project = new Entity\Project();
```

### 📖 Следует избегать использования пседонима (alias)
Они запутывают код и его понимание. Если у вас совпадают названия пространств имён, то, скорее всего, вы делаете что-то не так. Допустимо использовать псевдоним, если другое решение будет слишком сложным.

Bad:
```php
use Component\User;
use Entity\User as UserEntity;

$user = new UserEntity();
```

Good:
```php
use Component\User;
use Entity;

$user = new Entity\User();
```

**[⬆ up](#Summary)**

## **Functions**

### 📖 Должна быть использована максимально возможная типизация для вашей версии PHP. Все параметры и их типы должны быть описаны в PHPDoc. Возвращаемое значение тоже.

Bad:
```php
/**
 * @param $id
 * @param $name
 * @param $tags
 * @return mixed
 */
function storeUser($id, $name, $tags = []) {
    // ...
}
```

Good:
```php
// для PHP 7.1
/**
 * @param int $id
 * @param string $name
 * @param string[] $tags
 * @return User|null
 */
function storeUser(int $id, string $name, array $tags = []): ?User {
    // ...
}

// для PHP 5.6
// без строгой типизации возвращаемых типов любой метод
// может вернуть null, так что можно его не указывать в PHPDoc
/**
 * @param int $id
 * @param string $name
 * @param string[] $tags
 * @return User
 */
function storeUser($id, $name, array $tags = []) {
    // ...
}
```

### 📖 Все возможные типы должны быть определены в PHPDoc
Наибольшую пользу это приносит при работе с массивами:

Bad:
```php
/**
 * @param array $users
 * @param mixed $project
 * @param int $timestamp
 * @return mixed
 */ 
public function someMethod($users, $project, $timestmap) {
    foreach ($users as $user) {
        // IDE не сможет определить тип $user
    }
    // ...
}
```

Good:
```php
/**
 * @param Users[] $users
 * @param Project $project
 * @param int $timestamp
 * @return Foo
 */
public function someMethod(array $users, Project $project, int $timestmap): Foo {
    foreach ($users as $user) {
        // подсказки IDE и рефакторинг работают корректно
    }
    // ...
}
```

### 📖 В PHPDoc у аргументов не надо указывать `null`

Bad:
```php
/**
* @param string|null $sortBy
* @return \DateTimeZone[]
*/
public function getTimeZonesList($sortBy = null) {
    // ...
}
```

Good:
```php
/**
* @param string $sortBy
* @return \DateTimeZone[]
*/
public function getTimeZonesList($sortBy = null) {
    // ...
}
```

### 📖 В PHPDoc в возвращаемом значении не надо указывать `void` и `null`, если метод ничего не возвращает.

Bad:
```php
/**
 * @param string $controllerName
 * @return void
 */
public function runApplication(string $controllerName) {
    // ...
}

/**
 * @return null
 */
public function run() {
    // ...
}
```

Good:
```php
/**
 * @param string $controllerName
 */
public function runApplication(string $controllerName) {
    // ...
}

public function run() {
    // ...
}
```

### 📖 Название метода должно начинаться с глагола и соответствовать правилам именования переменных.

Bad:
```php
public function items() {
    // ...
}
public function convertedDataObject(array $data) {
    // ...
}
```

Good:
```php
public function loadItems() {
    // ...
}
public function convertDataToObject(array $data) {
    // ...
}
```

### 📖 Нельзя использовать глагол get в геттерах
Например, вместо `getDate()` следует писать `date()`. Геттер — метод, работающий только с полями своего объекта.

Bad:
```php
class User {
    private $_date;
    private $_customFields;

    public function getDate() {
        return $this->_date;
    }

    public function getCustomFields() {
        return json_decode($this->_customFields);
    }
}
```

Good:
```php
class User {
    private $_date;
    private $_customFields;

    public function date() {
        return $this->_date;
    }

    public function decodedCustomFields() {
        return json_decode($this->_customFields);
    }
}
```

### 📖 Методы названия, которых начинаются c `check` и `validate`, должны выбрасывать исключения и не возвращать значения

Bad:
```php
public function validateRequestData(array $requestData) {
    if (!array_key_exists('key', $requestData)) {
        return false;
    }
    // ...
    return true;
}
```

Good:
```php
public function validateRequestData(array $requestData) {
    if (!array_key_exists('key', $requestData)) {
        throw new ValidationError('Field "key" not found');
    }
    // ...
}
```

### 📖 Все методы класса по умолчанию должны быть private
Если метод используется наследниками класса, то он объявляется `protected`. Если используется сторонними классами, тогда `public`.

### 📖 Использование рекурсий допускается только в исключительном случае
Если код без рекурсии будет очень сложен для написания и понимания и при этом рекурсия гарантированно не выйдет за ограничения стека вызовов.

### 📖 Запрещается кешировать данные в статических переменных метода
Для кеширование в памяти используем свойство объекта.

Bad:
```php
public function loadData() {
    static $_cachedData;
    if ($_cachedData === null) {
        $_cachedData = [];
    }
    return $_cachedData;
}
```

Good:
```php
private $_cachedData = [];

public function loadData() {
    if ($this->_cachedData === null) {
        $this->_cachedData = [];
    }
    return $this->_cachedData;
}
```

### 📖 Параметры в методах должны следовать в следующем порядке: обязательные → часто используемые → редко используемые
Нужно соблюдать читаемость при написании вызова.

Bad:
```php
public function method($required, $practicallyUnused = 5, $often = [], $lessOften = null)
public function filter($value, $name, $operator) // ...$service->filter(15, "id", "=")
```

Good:
```php
public function method($required, $often = [], $lessOften = null, $practicallyUnused = 5)
public function filter($name, $operator, $value) // ...$service->filter("id", "=", 15)
```

### 📖 Если переменные, объявленные на вход к методу, могут быть `null`, они должны явно обозначаться как таковые

Good:
```php
/**
 * @param string $projectName
 */
public function someMethod($projectName = null) {
    // ...
}
```

**[⬆ up](#Summary)**

## **Return results from methods**

### 📖 Метод всегда должен возвращать только одну структуру данных (или `null`) или ничего не возвращать
Метод не может в разных ситуациях возвращать разные типы данных.

Bad:
```php
function loadUser() {
    if ($someCondition) {
        return ['id' => 1];
    }
    return new User();
}
```

Good:
```php
function loadUser() {
    if ($someCondition) {
        $user = new User();
        $user->id = 1;
        return $user;
    }
    return new User();
}
```

### 📖 Если метод возвращает один объект (или скалярный тип), то в случае, если объект не найден, возвращается `null`
Если же метод возвращает список объектов, то в случае, когда список пуст, возвращает пустой массив. Нельзя возвращать вместо пустого списка `null`.

Bad:
```php
function loadUsers() {
    if ($someCondition) {
        return null;
    }
    return [new User()];
}
```

Good:
```php
function loadUsers() {
    if ($someCondition) {
        return [];
    }
    return [new User()];
}
```

Однако, бывают ситуации, когда надо явно указать, что данные отсутвуют, а не содержат пустой список.

Пример: значения полей объекта задаются пользователем. Возможны две ситуации:
- пользователь не знает, каким категориям принадлежит объект — `null`
- пользователь знает, что объект не принадлежит ни одной категории — пустой массив (`[]`)

Тогда для получения категорий объекта будет правильным такой код:

```php
/**
 * для PHP 5.6
 * @return array|null
 */
function getObjectCategories($object) {
    if ($object->categories === null) {
        return null;
    }
    return parseCategories($object->categories);
}

/**
 * для PHP 7.1
 * @return array|null
 */
function getObjectCategories($object): ?array {
    if ($object->categories === null) {
        return null;
    }
    return parseCategories($object->categories);
}
```

### 📖 Возвращаемая переменная обычно `$result`
Если у вас метод `loadUsers`, то не надо внутри метода возвращаемую переменную называть `$users`. В любом месте в методе должно быть понятно, где вы оперируете результатом, а где локальными переменными.

Bad:
```php
function loadUsers() {
    $users = [];
    // ...
    foreach ($data as $item) {
        $users[] = new User();
    }
    return $users;
}
```

Good:
```php
function loadUsers() {
    $result = [];
    // ...
    foreach ($data as $item) {
        $result[] = new User();
    }
    return $result;
}
```

### 📖 Метод должен явно отличать нормальные ситуации от исключительных
Если никакой ошибки не произошло, но отсутствует результат, то это `null` (или пустой массив), однако если все же произошла исключительная ситуация, которая не заложена системой, то должно кидаться исключение.

Bad:
```php
function loadUsers() {
    if ($connectionError !== null) {
        return []; // потеряли ошибку, никто не узнает о проблемах с подключением
    }
    // ...
    if (count($data) === 0) {
        return [];
    }
    // ...
    return $result;
}
```

Good:
```php
function loadUsers() {
    if ($connectionError !== null) {
        throw new Exception\ConnectionError();
    }
    // ...
    if (count($data) === 0) {
        return [];
    }
    // ...
    return $result;
}
```

### 📖 Метод должен придерживаться следующей структуры: Проверка параметров → Получение данных → Работа → Результат
Во время проверки параметров и получения необходимых данных метод должен возвращать соответствующее пустое значение или кидать исключение. После того как метод получил все необходимые данные и приступил к работе выход из метода крайне нежелателен. Возможны редкие исключения, облегчающие понимание и читаемость кода.

Bad:
```php
/**
 * @return int
 */
public function someMethod() {
    $isValid = $this->_someCheck();
    if ($isValid) {
        $tmp = 0;
        $someValue = $this->_getSomeValue();
        if ($someValue > 0) {
            $tmp = $someValue;
        }
        $anotherValue = $this->_getAnotherValue();
        if ($anotherValue > 0) {
            return $tmp + $anotherValue;
        } else {
            return $someValue;
        }
    } else {
        throw new \Exception('Invalid condition');
    }
}
```

Good:
```php
/**
 * @return int
 * @throws \Exception
 */
public function someMethod() {
    $result = 0;
     
    $isValid = $this->_someCheck();
    if (!$isValid) {
        throw new \Exception('Invalid condition');
    }
  
    $someValue = $this->_getSomeValue();
    if ($someValue > 0) {
        $result += $someValue;
    }
    $anotherValue = $this->_getAnotherValue();
    if ($anotherValue > 0) {
        $result += $anotherValue;
    }
    return $result;
}
```

**[⬆ up](#Summary)**

## **Classes**

### 📖 Трейты имеют постфикс Trait

Good:
```php
trait AjaxResponseTrait {
    // ...
}
```

### 📖 Интерфейсы имеют постфикс Interface

Good:
```php
interface ApplicationInterface {
    // ...
}
```

### 📖 Абстрактные классы имеют префикс Abstract

Good:
```php
abstract class AbstractApplication {
    // ...
}
```

### 📖 Все свойства класса по умолчанию должны быть private
Если свойство используется наследниками класса, то оно объявляется `protected`. Если используется сторонними классами, тогда `public`.

Bad:
```php
abstract class Loader {
    public $data = [];

    public function getData() {
        return $this->data;
    }

    public function init() {
        $this->data = $this->load();
    }

    abstract public function load();
}
```

Good:
```php
abstract class Loader {
    
    /**
     * @type array
     */
    private $_cachedData = [];

    /**
     * @return array
     */
    public function getData(): array {
        return $this->_cachedData;
    }

    public function init(): void {
        $this->_cachedData = $this->_load();
    }

    /**
     * @return array
     */
    abstract protected function _load(): array;
}
```

### 📖 Методы и свойства в классе должны быть отсортированы по уровням видимости и по порядку использования сверху вниз
Уровни видимости: `public` -> `protected` -> `private`.

Bad:
```php
class SomeClass {
    private $_privPropA;   
    public $pubPropA;
    protected $_protPropA;
 
    protected function _protA() {
    }
 
 
    public function pubB() {
    }
 
 
    private function _privA() {
        return $this->_protA();
    }
  
    public function pubA() {
        $this->_privA();
        return $this->pubB();
    }
}
```

Good:
```php
class SomeClass {
    public $pubPropA;
    protected $_protPropA;
    private $_privPropA;
 
    public function pubA() {
        $this->_privA();
        return $this->pubB();
    }
 
    public function pubB() {
    }
 
    protected function _protA() {
    }
 
    private function _privA() {
        return $this->_protA();
    }
}
```

**[⬆ up](#Summary)**

## **Objects**

### 📖 Все объекты должны быть неизменяемыми (immutable), если от них не требуется обратного

Bad:
```php
class SomeObject {
    /**
     * @var int
     */
    public $id;
}
```

Good:
```php
class SomeObject {
    /**
     * @var int
     */ 
    private $_id;
  
    /**
     * @param int $id
     */
    public function __construct($id) {
        $this->_id = $id;
    }
  
    /**
     * @var int
     */
    public function id() {
        return $this->_id;
    }
}
```

### 📖 Статические вызовы можно делать только у самого класса. У экземпляра можно обращаться только к его свойствам и методам

Bad:
```php
$type = $user::TYPE;
```

Good:
```php
$type = User::TYPE;
```

**[⬆ up](#Summary)**

## **Comments**
### 📖 В общем случае комментарии запрещены

Желание добавить комментарий — признак Bad читаемого кода. Любой участок кода, который вы хотели бы выделить или прокомментировать, надо выносить в отдельный метод.

Фразу, которую вы хотели написать в комментарии, надо привести в простой вид и сделать ее названием метода.

Bad:
```php
public function deleteApprovedUsers() {
    // load users filter them by approval
    $users = $repository->loadUsers();
    array_filter($users, function($user) {
        return $user->is_approved;
    });

    foreach ($users as $user) {
        // ...
    }
}
```

Good:
```php
public function deleteApprovedUsers() {
    $users = $this->loadApprovedUsers();
    foreach ($users as $user) {
        // ...
    }
}

public function loadApprovedUsers() {
    $users = $repository->loadUsers();
    array_filter($users, function($user) {
        return $user->is_approved;
    });
}
```

### 📖 Вынужденные хаки должны быть помечены комментариями
Лучше соблюдать одинаковый формат в рамках проекта

Good:
```php
function loadUsers() {
    $result = $repository->loadUsers();
    // hack: status field was removed from storage 
    foreach ($result as $user) {
        $user->status = 'active';
    }
    // hack end
    return $result;
}
```

### 📖 Готовые алгоритмы, взятые из внешнего источника, должны быть помечены ссылкой на источник

Good:
```php
/**
 * https://en.wikipedia.org/wiki/Quicksort
 */
function quickSort(array $arr) {
    // ...
}

/**
 * https://habrahabr.ru/post/320140/
 */
function generateRandomMaze() {
    // ...
}
```

### 📖 При разработке прототипа допустимо помечать участки кода @todo

Good:
```php
function loadUsers() {
    $result = $repository->loadUsers();
    // @todo: delete the hack when field will be restored
    // hack: status field was removed from storage
    foreach ($result as $user) {
        $user->status = 'active';
    }
    // hack end
    return $result;
}
```

**[⬆ up](#Summary)**

## **Exceptions**
### 📖 На каждом уровне бизнес-логики (проект, компонент, библиотека) должно быть абстрактное базовое исключение

### 📖 Исключения сторонних библиотек должны быть перехвачены сразу
Далее либо обработаны, либо на их основании должно бросаться свое исключение. Новое исключение должно содержать предыдущее.

Good:
```php
namespace Service\Facebook;

use Exception;
use FacebookAds;

public function function requestData() {
    // ...
    try {
        $objects = $facebookAds->requestData($params);
    } catch (FacebookAds\Exception\Exception $e) {
        throw new Exception\ExternalServiceError("Facebook error", 0, $e);
    }
    //..
}
```

### 📖 По умолчанию тексты исключений не должны показываться пользователю
Они предназначены для логирования и отладки. Текст исключения можно показать пользователю, если оно явно для этого предназначено: например, реализует интерфейс `HumanReadableInterface`.

```php
interface HumanReadableInterface {
    
    /**
     * @return string
     */
    public function getUserMessage(): string;
}

public function handleException(\Throwable $exception) {
    if ($exception instanceof HumanReadableInterface) {
        echo $exception->getUserMessage();
        return;
    }
    // ...
}
```

**[⬆ up](#Summary)**

## **Cloud**

### 📖 Нельзя делать запросы к внешнему хранилищу внутри цикла с заведомо большим кол-вом итераций

Bad:
```php
$users = loadUsers();
foreach ($users as $user) {
    $userProjects = loadUserProjects($user);
    // ...
}
```

Good:
```php
$users = loadUsers();
$projects = loadProjects();
$indexedProjects = [];
foreach ($projects as $project) {
    if (!array_key_exists($project->user_id, $indexedProjects)) {
        $indexedProjects[$project->user_id] = [];
    }
    $indexedProjects[$project->user_id][] = $project;
}
foreach ($users as $user) {
    if (!array_key_exists($user->id, $indexedProjects)) {
        continue;
    }
    $userProjects = $indexedProjects[$user->id];
}
```

### 📖 Для каждой записи в хранилище должно быть понятна дата ее создания
То есть должна быть колонка `date/creation_date`. Или должен быть зависимый объект (связь 1 к 1), у которого есть такая колонка. Редактируемые записи должны иметь и дату редактирования: `update_date` или `modification_date`.

**[⬆ up](#Summary)**

## **Pull Requests (PR)**
### 📖 PR должен содержать как можно меньше строк кода
Любая атомарная часть кода должна выделяться в отдельную подзадачу и отдельный PR.

### 📖 Нельзя смешивать перенос методов в другие классы и места и последующий рефакторинг между собой
Перенос методов в другие классы и места должны быть выделены в отдельный PR. Последующий рефакторинг после переноса тоже должен быть в отдельном PR.

### 📖 В случае большого PR — ответственность за долгий просмотр несет сам разработчик, сделавший такой PR
Нормальный объем кода — 0-300 строк в зависимости от его сложности. PR заглушек и архитектуры может содержать много формального кода, который легко быстро проверить. PR же конкретного метода может содержать много сложностей даже в 10 строчках. 

### 📖 Нельзя накапливать изменения в какой-то своей ветке и потом делать большой PR в master
Все что можно смержить в master без последствий (даже если это еще не готовый результат, а только заглушки или часть, но они скрыты от юзеров и никому не мешают), должен мержиться в master и PR должен создаваться в master.

### 📖 В Pull Request не должно попадать кода, не относящегося к задаче
Также не должно быть забытых комментариев, бессмысленных переносов строк и прочего "строительного мусора". Каждое изменение, которое вы предлагаете сделать в master-ветке, должно так или иначе относиться к решению поставленной вам задачи.

**[⬆ up](#Summary)**

## **Templates**

### 📖 В шаблонах не должны вызываться методы объектов (геттеры не в счет)
Все необходимые данные должны быть загружены до рендера и переданы в виде параметров шаблона.

**[⬆ up](#Summary)**

## **Работа с литералами**

### 📖 Назначение всех числовых литералов должно быть понятным из контекста
Они должны быть или вынесены в переменную или константу, или сравниваться с переменной, или передаваться на вход методу с понятной сигнатурой. В коде должен присутствовать в явном виде ответ: `за что отвечает это число и почему оно именно такое?`

Bad:
```php
$isOnlyDeleted = 1;
if ($object->is_deleted === $isOnlyDeleted) {
    // ...
}
 
for ($i = 0; $i < 5; $i++) {
    // ...
}
```

Good:
```php

if ($object->is_deleted === 1) {
    // ...
}
 
$apiMaxRetryLimit = 5;
for ($i = 0; $i < $apiMaxRetryLimit; $i++) {
    // ...
} 
```

**[⬆ up](#Summary)**

## **Conditions**

### 📖 В условном операторе должно проверяться исключительно `boolean` значение

Bad:
```php
if (count($userProjects)) {
    // ...
}
if ($project) {
    // ...
}
```

Good:
```php
if ($isResponseError) { // $isResponseError = true
    // ...
}
if ($response->isError()) { // isError method returns boolean
    // ...
}
if (count($userProjects) > 0) {
    // ...
}
```

### 📖 В сравнении не boolean переменных используется строгое сравнение с приведением типа (===), автоматическое приведение и нестрогое сравнение не используются

Bad:
```php
if ($project) {
    // ...
}
if ($request->postData('sum') == 100) {
    // ...
}
if (!$request->postData('sum')) {
    // ...
}
if (!$bill->comment) {
    // ...
}
```

Good:
```php
if ($project === null) { // $project is an object
    // ...
}
if ((int)$request->postData('sum') === 100) {
    // ...
}
if ($bill->comment === '') {
    // ...
}
```

### 📖 Автоматическое приведение типов разрешено только, когда один из операндов — литерал с зфиксированным типом
При сравнении двух переменных с неизвестными типами для читающего код человека не очевидно, к чему они будут приведены интерпретатором. Если же тип одного из операндов известен, то всё становится очевидно и ручное приведение типов не требуется.

Если вы хотите проверить значение `boolean` пришедшее извне, то делается это так:

Bad:
```php
if ((int)$request->get('is_something') > 0) {
    // ...
}
if ((int)$request->get('is_something') === 1) {
    // ...
}
if ((int)$user->is_registered === 0) {
    // ...
}
```

Good:
```php
if ($request->get('is_something') > 0) {
    // ...
}
if ($user->is_registered) {
    // ...
}
if (!$user->is_registered) {
    // ...
}
```

#### 📖 Не надо сравнивать `boolean` с `true`/`false`
Это нарушает запрет на бесполезный код.

Bad:
```php
if ($bill->isPaid() == true) {
    // ...
}
if ($bill->isPaid() !== false) {
    // ...
}
if (!$bill->isPaid() === true) {
    // ...
}
if (!(!$bill->isPaid() === true)) {
    // ...
}
if ((bool)$phone->is_external === true) {
    // ...
}
```

Good:
```php
if ($bill->isPaid()) {
    // ...
}
```

### 📖 Проверять переменные надо на наличие позитивного вхождения, а не отсутствие негативного
Если вам нужна строка, то проверять надо на то, что переменная является строкой. Не надо проверять на то, что она не является числом или чем-то еще. Перечислять все возможные варианты, чем переменная не должна быть, значит повышать риск ошибки и усложнять поддержку кода.

Bad:
```php
if (!is_numeric($value) && !is_object($value)) {
    // ...
}
```

Good:
```php
if (is_string($value) && $value !== '') {
    // ...
}
```

### 📖 If you use the built-in PHP function, that returns `0`, `1` and, maybe, `false`, then if possible use the result in a  `bool` condition without additional checks
This is not for the case in which you have to handle two results differently, for example `0` and `false`.

Bad:
```php
if (preg_match($pattern, $subject) === 1) {
    // ...
}
if (!strpos($search, $text)) {
    // ...
}
```

Good:
```php
if (preg_match($pattern, $subject)) { 
    // handle success
}
if (!preg_match($pattern, $subject)) {
    // handle not success
}
if (preg_match($pattern, $subject) === false) {
    // handle error
}
if (strpos($search, $text) === false) {
    // handle not success
}
```

### 📖 Using both AND and OR operators in same expression requires direct priority indication with braces
Note the different meanings of two "good" versions

Bad:
```php
if ($isMobile || $isSizeTooBig && $isAllowedToShrink) {
    // ...
}
```

Good:
```php
if (($isMobile || $isSizeTooBig) && $isAllowedToShrink) {
    // ...
}
if ($isMobile || ($isSizeTooBig && $isAllowedToShrink)) {
    // ...
}
```

**[⬆ up](#Summary)**

## **Ternary Operators**

### 📖 Ternary operator is used with the same rules as in conditions

### 📖 Ternary operator is used, if the variable is defined the same way in both "if" cases
При наличии логики в ветках условия следует рассмотреть возможность вынести ее в отдельный метод.

Bad:
```php
if ($isExternal) {
    $bill = $this->loadExternalBill();
} else {
    $bill = $this->loadInternalBill();
}
```

Good:
```php
$bill = $isExternal ? $this->loadExternalBill() : $this->loadInternalBill();
```

### 📖 The usage of ternary operator `?:` can be used if the default value is defined

Bad:
```php
$contact = $this->loadContactByPhone() ?: $this->loadContactByEmail() ?: $this->loadContactByName();
```

Good:
```php
$lead = $this->loadLeadFromCache() ?: $this->loadLeadFromDB();
$contact = $this->loadContactByPhone() ?: $this->loadContactByEmail() ?: $this->loadContactByName() ?: null;
```

**[⬆ up](#Summary)**

## **Tests**

### 📖 Tests is also production-code, as well as other code
They have to be written following the rules described in this document.

### 📖 В дата провайдерах для тестов надо писать комментарий к структуре отдаваемого массива значений

Bad:
```php
public function isEmailAddressData() {
    return [
        ['test@test.ru',            true ],
        // ...
    ]
}
```

Good:
```php
public function isEmailAddressData() {
    return [
        //    email               isValid
        ['test@test.ru',            true ],
        ['invalidEmail',            false],
        // ...
    ]
}
```

**[⬆ up](#Summary)**

## **Chaining Objects**
### 📖 A method with many non-required params (А) may be replaced with a chain-object
The constructor gets all the required params, and others are added with setters, that are returning the self object (chaining). The object has one verb-method without parameters, he finishes the object use и do the actions usually made by the method А.

**Before:**
```php
function send($method, $url, $body = null, $headers = null, $retries = 1, $timeout = 300) {}
```

**Has to be chained:**
```php
public function __construct($method, $url) {
    // ...
}

public function body($body) {
    return $this;
}

public function send();
```

**Usage of a new object:**
```php
new $sender($method, $url)->body($body)->retries(10)->timeout(25)->send();
```

**[⬆ up](#Summary)**

## **Cron Jobs and Scripts**
### 📖 Any script, that makes changes in the data, have to ask the confirmation and `debug` the results

Bad:
``` php
// cli/delete_items.php
$repository->deleteItems();
```

Fix that, so that an accidental launch wouldn't do any damage:

Good:
```php
// cli/delete_items.php
$totalItems = $repository->countItems();
if (!confirm("Do you want to delete {$totalItems} item(s)?")) {
    echo 'Delete canceled, exit';
    exit(1);
}

$repository->deleteItems();

function confirm(string $question): bool {
    return readline("{$question} [y/n]: ") === 'y'
}
```

**[⬆ up](#Summary)**

## **Authors**

- Удодов Евгений ([flrnull](https://github.com/flrnull))
- Рудаченко Сергей ([m1nor](https://github.com/m1nor))
- Зюзькевич Юрий ([Farengier](https://github.com/Farengier))
