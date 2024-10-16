---
demo:
  module: Module 05 - Load balancing HTTPS traffic
  title: Anwendungsgateway (Modul 05)
---
## Konfigurieren von Azure Application Gateway

In dieser Demonstration lernen Sie, wie Sie eine Azure Application Gateway-Instanz erstellen. 

**Simulation:**[ Bereitstellen eines Azure Application Gateway](https://mslabs.cloudguides.com/guides/AZ-700%20Lab%20Simulation%20-%20Deploy%20Azure%20Application%20Gateway)

**Referenz:** [Schnellstart: Weiterleiten von Webdatenverkehr per Azure Application Gateway – Azure-Portal](https://learn.microsoft.com/azure/application-gateway/quick-create-portal)

**Referenz**: [End-to-End-Verschlüsselung des Netzwerkdatenverkehrs mit Azure Application Gateway](https://github.com/MicrosoftDocs/mslearn-end-to-end-encryption-with-app-gateway)

**Hinweis:** Erstellen Sie der Einfachheit halber neue virtuelle Netzwerke und Subnetze, während Sie die Konfiguration durchlaufen. 

**Erstellen Sie die Azure Application Gateway-Instanz.**

1. Öffnen Sie das Azure-Portal.

1. Suchen und wählen Sie **Azure Application Gateway** aus.

1. **Erstellen** Sie ein neues Gateway.

1. Besprechen Sie auf der Registerkarte **Grundeinstellungen** die Optionen **Tarife**, **Automatische Skalierung** und **Instanzen**.

1. Besprechen Sie auf der Registerkarte **Front-Ends** die IP-Adresstypen.

1. Besprechen Sie auf der Registerkarte **Back-Ends**, wann ein leerer Back-End-Pool verwendet werden soll.

1. Besprechen Sie auf der Registerkarte **Konfiguration** die Routingregeln. Vergleichen Sie diese mit den Lastenausgleichsregeln.

1. Erläutern Sie, dass nach dem Erstellen des Gateways die Back-End-Ziele hinzugefügt und getestet werden sollten. 
