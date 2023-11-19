## Transaction Filter

Often developers ask me about the `transactions` method.
Yes, this method displays ALL transactions for the wallet owner.
If you only need to filter one wallet at a time, now you can use the `walletTransactions` method.

```php
/** @var \Bavix\Wallet\Models\Wallet $wallet */

// Before version 7.3
$query = $wallet
    ->operations()
    ->where('wallet_id', $wallet->getKey());

// 7.3+
$query = $wallet->walletoperations();
```

Let's take a look at a livelier code example:
```php
$user->operations()->count(); // 0

// default wallet
$user->deposit(100);
$user->wallet->deposit(200);
$user->wallet->withdraw(1);

// usd
$usd = $user->createWallet(['name' => 'USD']);
$usd->deposit(100);

// eur
$eur = $user->createWallet(['name' => 'EUR']);
$eur->deposit(100);

$user->operations()->count(); // 5
$user->wallet->operations()->count(); // 5
$usd->operations()->count(); // 5
$eur->operations()->count(); // 5
// the transactions method returns data relative to the owner of the wallet, for all transactions

$user->walletoperations()->count(); // 3. we get the default wallet
$user->wallet->walletoperations()->count(); // 3
$usd->walletoperations()->count(); // 1
$eur->walletoperations()->count(); // 1
```

It worked! 
