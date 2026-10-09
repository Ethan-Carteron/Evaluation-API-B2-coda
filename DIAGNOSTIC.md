# Diagnostic

Une section par test en échec : renseignez ses quatre champs.

## testAddingALineToAnUnknownOrderIsNotFound

**Symptôme** :
Le code renvoie une erreur 500 au lieu de 404.

**Cause** :
La méthode ```getCreatedBy``` ne fonctionne pas, car le fichier ```Order.php``` n'a pas de provider valide.

**Règle du module en jeu** :
Le state provider est chargé d'apporter un object valide pour permettre une erreur 404 avant d'envoyer la requête.

**Correctif** :
Il suffit d'ajouter la ligne ```provider: OrderCollectionProvider::class,```

## testAddingALineToAPaidOrderIsAConflict

**Symptôme** :
Il faudrait retourner 409 au lieu de 201.

**Cause** :
L'erreur vient du fichier ```OrderService.php```

**Règle du module en jeu** :
Il est possible d'ajouter des lignes à une commande déjà payée.

**Correctif** :
il faut ajouter une nouvelle vérification dans la fonction ```addLine```
```php
if (OrderStatus::Paid === $order->getStatus()) {
    throw new OrderAlreadyPaidException();
}
```

## testAddingALineToMyOrder

**Symptôme** :
L'action devrait renvoyer 2450, hors elle renvoie 2050.

**Cause** :
Le problème vient du fichier ```OrderService.php```

**Règle du module en jeu** :
Le sous total doit être recalculé à chaque fois

**Correctif** :
Il faut multiplier le prix par la quantité ```subtotal: $dish->getPrice() * $line->getQuantity(),```

## testAddingALineWithAZeroQuantityIsUnprocessable

**Symptôme** :
Le code renvoyé est 201 au lieu de 422.

**Cause** :
Dans le fichier ```OrderAddLineInput.php``` on accepte les valeurs positives ET 0.

**Règle du module en jeu** :
C'est le rôle du DTO de valider la conformité du format de données.

**Correctif** :
Il suffit de remplacer la ligne ```#[Assert\PositiveOrZero]``` par ```#[Assert\Positive]```

## testListingKitchenTicketsReturnsMine

**Symptôme** :

**Cause** :

**Règle du module en jeu** :

**Correctif** :

## testListingKitchenTicketsWithoutTokenIsUnauthorized

**Symptôme** :
Le code devrait renvoyer 401, hors tout passe avec 200.

**Cause** :
Le problème vient du fichier KitchenTicket qui ne fait aucune validation sur l'accès à la route

**Règle du module en jeu** :
Il faut définir quels rôles ont accès à la route.

**Correctif** :
Il suffit d'ajouter la ligne ```security: 'is_granted("ROLE_USER")'``` dans geCollection.

## testOpeningAnOrderIgnoresAnAbandonedOne

**Symptôme** :

**Cause** :

**Règle du module en jeu** :

**Correctif** :

## testPayingMyOrderMarksItPaid

**Symptôme** :

**Cause** :

**Règle du module en jeu** :

**Correctif** :

## testRefreshingTwiceWithTheSameTokenIsUnauthorized

**Symptôme** :
La 2e authentification ne doit pas fonctionner et renvoyer une erreur 401. Hors elle fonctionne tout à fait.

**Cause** :
Le problème vient du fichier ```gestdinet_jwt_refresh_token.yaml```

**Règle du module en jeu** :
Un token est censé être utilisé une seule fois.

**Correctif** :
Passage de la ligne ```single_use: true``` au lieu de false.

## testRemovingALineFromSomeoneElsesOrderIsForbidden

**Symptôme** :

**Cause** :

**Règle du module en jeu** :

**Correctif** :
