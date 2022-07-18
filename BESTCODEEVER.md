```
<?php

namespace App\Listener\Sale;

use App\Entity\Sale\Cart;
use App\Entity\Sale\CartProduct;
use App\Sale\Cart\Service\CartService;
use Doctrine\Bundle\DoctrineBundle\Attribute\AsEntityListener;
use Doctrine\ORM\Event\LifecycleEventArgs;
use Doctrine\ORM\Events;

#[AsEntityListener(event: Events::preUpdate, entity: Cart::class)]
#[AsEntityListener(event: Events::preUpdate, entity: CartProduct::class)]
class CartChangeListener
{
    public function __construct(private readonly CartService $cartService)
    {
    }

    public function preUpdate(object $cart, LifecycleEventArgs $eventArgs)
    {
        if (!$cart instanceof Cart) {
            $cart = $cart->getCart();
        }

        $this->cartService->recalculate($cart);
    }
}
```