# Les tests unitaires

## Deux fichiers : le fichier à tester et son test

### test/Operation.php
```php
class Operation
{
    public function isEven(int $nb): bool
    {
        return ($nb % 2) === 0;
    }
}
```

### test/OperationTest.php
```php
class OperationTest extends Trucmuche
{
    public function testEven()
    {
        $op = new Operation();
        // méthode vérifiant si le résultat de la méthode isEven() avec le paramètre 4 vaut true
        $this->assertTrue($op->isEven(4));
    }
}
```

## Exemple plus concret
```php
use PHPUnit\Framework\TestCase;

class OperationTest extends TestCase
{
    public function testEven()
    {
        $operation = new Operation;
        $this->assertTrue($operation->isEven(2));
        $this->assertFalse($operation->isEven(3));
    }

    public function testCalc()
    {
        $operation = new Operation;
        $this->assertEquals(5, $operation->addition(2, 3));
        $this->assertEquals(6, $operation->multiplication(2, 3));
    }
}
```

## Exemples d'assertions
- assertEquals();
- assertTrue();
- assertFalse();
- assertGreaterThan();
- assertLesserThan();
- assertCount();
- assertContains();
...

[Liste complète](https://phpunit.de/manual/6.5/en/appendixes.assertions.html)