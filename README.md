# CakePHP MultiValidation behavior

A CakePHP 2.x behavior that allows easy modifying of validation rules on the fly.
Includes test cases.

* Author: QTSdev / www.qtsdev.com
* Version: 1.0
* CakePHP: 2.x 

## How to include

You can just copy the behavior file to your APP/Model/Behavior folder  

or

You can also install the plugin as with every other plugin:

* Put the files in `APP/Plugin/MultiValidation`
* In your bootstrap.php: `CakePlugin::load('MultiValidation')` or `CakePlugin::loadAll()` 

## How to use

Enable the plugin in your target model

```php
class User extends AppModel {
    
    public $actsAs = array(
        'MultiValidation.MultiValidation' => array(
            'types' => array(
                'enforceUsernameAndEmail' => array(
                    'username' => array(
                        'onlyAlpha' => array(
                            'rule' => 'alphaNumeric'
                        ),
                    ),
                    'email' => 
                        'validEmail' => array(
                            'rule' => 'email',
                            'required' => true
                        ),
                    )
                ),
                'enforcePassword' => array(
                    'password' => array(
                        'minCharsAllowed' => array(
                            'rule' => array('minLength', 8)
                        ),
                    ),
                ),
            )
        )
    )
    
    public $validate = array(
        'username' => array(
            'mustNotBeBlank' => array(
                'rule' => 'notEmpty'
            )
        ),
        'password' => array(
            'minCharsAllowed' => array(
                'rule' => array('minLength', 4)
            )
        ),
    );
}
```

Then in your model or controller you can do the following actions:

```php
    // Add new username and email validation:
    $User->addValidation('enforceUsernameAndEmail');
    // A call to $User->loadedValidation() returns: 
    // array('enforceUsernameAndEmail')
    
    // Reset to default state:
    $User->resetValidation();
    // A call to $User->loadedValidation() returns: 
    // array('_default')
    
    // Load and set only the password validation:
    $User->loadValidation('enforcePassword');
    // A call to $User->loadedValidation() returns: 
    // array('enforcePassword')
    
    // Add the other validation type also:
    $User->addValidation('enforceUsernameAndEmail');
    // A call to $User->loadedValidation() returns: 
    // array('enforcePassword', 'enforceUsernameAndEmail')
    
    // Reset again to default state:
    $User->resetValidation();
    // A call to $User->loadedValidation() returns: 
    // array('_default')
```
