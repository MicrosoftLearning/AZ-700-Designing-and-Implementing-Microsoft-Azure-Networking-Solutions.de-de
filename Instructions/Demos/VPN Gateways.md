---
demo:
  title: VPN-Gateways
  module: Module 02 - Design and implement hybrid networking
---
In dieser Dem untersuchen wir Gateways für virtuelle Netzwerke.

**Hinweis:**  Diese Demonstration funktioniert am besten mit zwei virtuellen Netzwerken mit Subnetzen.

## Erkunden des Bereichs „Gatewaysubnetz“
1. Wählen Sie für eines Ihrer virtuellen Netzwerke das Blatt  **Subnetze**  aus.
1. Wählen Sie  **+ Gatewaysubnetz** aus. Beachten Sie, dass der Name des Subnetzes nicht geändert werden kann. Sehen Sie sich den  **Adressbereich**  des Gatewaysubnetzes an. Die Adresse muss innerhalb des Adressraums des virtuellen Netzwerks liegen.
1. Beachten Sie, dass jedes virtuelle Netzwerk ein Gatewaysubnetz benötigt.
1. Schließen Sie die Seite „Gatewaysubnetz hinzufügen“. Ihre Änderungen müssen nicht gespeichert werden.

## Erkunden des Bereichs „Verbundene Geräte“
1. Wählen Sie für das virtuelle Netzwerk das Blatt  **Verbundene Geräte**  aus.
1. Nach der Bereitstellung eines Gatewaysubnetzes wird es in der Liste der verbundenen Geräte angezeigt.

## Hinzufügen einer VNet Gateway-Instanz
1. Suchen Sie nach  **Gateways für virtuelle Netzwerke**.
1. Klicken Sie auf  **+ Hinzufügen**.
1. Sehen Sie sich die einzelnen Einstellungen für das VNET-Gateway an.
1. Über die Informationssymbole erhalten Sie weitere Informationen zu den Einstellungen.
1. Sehen Sie sich  **Gatewaytyp**,  **VPN-Typ** und  **SKU** an.
1. Beachten Sie, dass eine Angabe unter  **Öffentliche IP-Adresse** erforderlich ist.
1. Denken Sie daran, dass jedes virtuelle Netzwerk über ein Gateway für virtuelle Netzwerke verfügen muss.
1. Schließen Sie die Option zum Hinzufügen eines VNET-Gateways. Ihre Änderungen müssen nicht gespeichert werden.
   
## Hinzufügen einer Verbindung zwischen den virtuellen Netzwerken
1. Suchen Sie nach  **Verbindungen**.
1. Klicken Sie auf  **+ Hinzufügen**.
1. Beachten Sie, dass für  **Verbindungstyp**  die folgenden Optionen festgelegt werden können: „VNet-zu-VNet“, „Standort-zu-Standort (IPsec)“ oder „ExpressRoute“.
1. Geben Sie die entsprechenden Informationen an, und klicken Sie auf die Schaltfläche  **OK** .
1. Auf der Seite  **Einstellungen**  müssen Sie zwei unterschiedliche virtuelle Netzwerke auswählen.
1. Lesen Sie die Hilfeinformationen zum Kontrollkästchen  **Bidirektionale Konnektivität einrichten** .
1. Sehen Sie sich die Informationen unter  **Gemeinsam verwendeter Schlüssel (PSK)**  an.
1. Schließen Sie die Seite „Verbindung hinzufügen“. Ihre Änderungen müssen nicht gespeichert werden.
