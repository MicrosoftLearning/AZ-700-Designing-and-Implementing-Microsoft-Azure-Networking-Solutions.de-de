---
demo:
  title: Load Balancer
  module: 'Module 04 - Load balancing non-HTTPS traffic '
---
## Konfigurieren von Azure Load Balancer

In dieser Demonstration erfahren Sie, wie Sie einen öffentlichen Lastenausgleich erstellen. 

**Referenz:** [Schnellstart: Erstellen eines öffentlichen Lastenausgleichs für den Lastenausgleich virtueller Computer über das Azure-Portal](https://learn.microsoft.com/azure/load-balancer/quickstart-load-balancer-standard-public-portal)

**Hinweis:** Für diese Demo ist ein virtuelles Netzwerk mit mindestens einem Subnetz erforderlich. 

**Zeigen Sie das Feature „Auswahlhilfe“ des Portals.**

1. Öffnen Sie das Azure-Portal.

1. Suchen Sie nach Lastenausgleich, und wählen Sie die Option **Lastenausgleich > Auswahlhilfe** aus.

1. Verwenden Sie den Assistenten, um verschiedene Szenarios durchzugehen.
   
**Erstellen eines Lastenausgleichs**

1. Fahren Sie im Azure-Portal fort.

1. Suchen Sie nach **Lastenausgleich**, und wählen Sie diese Option aus. **Erstellen** Sie einen Lastenausgleich. 

1. Besprechen Sie auf der Registerkarte **Grundeinstellungen** die Angaben **SKU**, **Typ** und **Tarif**.

1. Besprechen Sie auf der Registerkarte **Front-End-IP-Konfiguration** die Verwendung einer öffentlichen IP-Adresse.

1. Wählen Sie auf der Registerkarte **Back-End-Pools** das virtuelle Netzwerk mit IP-Adressbereich aus.

1. Erstellen Sie auf der Registerkarte **Eingangsregeln** eine Lastenausgleichsregel. Besprechen Sie Parameter wie **Protokoll**, **Ports**, **Integritätstests** und **Sitzungspersistenz**. 


