# Webservice-1.5-csharp

Integração em C# com o Webservice Cielo 1.5

## Criando uma transação

```csharp
String mid = "1006993069";
String key = "25fbb99741c739dd84d7b06ec78c9bac718838630f30b112d033ce2e621b34f3";

Cielo cielo = new Cielo (mid, key, Cielo.TEST);

Holder holder = cielo.holder ("4012001038443335", "2018", "05", "123");
holder.name = "Fulano Portador da Silva";

Random randomOrder = new Random ();

Order order = cielo.order (randomOrder.Next (1000, 10000).ToString (), 10000);
PaymentMethod paymentMethod = cielo.paymentMethod (PaymentMethod.VISA, PaymentMethod.CREDITO_A_VISTA);

Transaction transaction = cielo.transactionRequest (
	holder,
	order,
	paymentMethod,
	"http://localhost/cielo",
	Transaction.AuthorizationMethod.AUTHORIZE_WITHOUT_AUTHENTICATION,
	false
);

//...
```

## Criando uma transação e enviando para autorização

```csharp
//...

try {
	Transaction transaction = cielo.transactionRequest (
		                          holder,
		                          order,
		                          paymentMethod,
		                          "http://localhost/cielo",
		                          Transaction.AuthorizationMethod.AUTHORIZE_WITHOUT_AUTHENTICATION,
		                          false
	                          );

	if (transaction.authorization != null && transaction.authorization.lr == 0) {
		Console.Write ("Transação autorizada com sucesso. TID=");
		Console.WriteLine (transaction.tid);
	}
} catch (Exception e) {
	Console.WriteLine ("Opz..");
	Console.WriteLine (e.Message);
}
```

## Cancelamento total de uma transação

```csharp
//...


try {
	transaction = cielo.cancellationRequest (transaction);
} catch (Exception e) {
	// ...
}
```

## Captura total de uma transação

```csharp
//...


try {
	transaction = cielo.captureRequest (transaction);
} catch (Exception e) {
	// ...
}
```

## Captura parcial de uma transação

```csharp
//...


try {
	transaction = cielo.captureRequest (transaction, 10000);
} catch (Exception e) {
	// ...
}
```