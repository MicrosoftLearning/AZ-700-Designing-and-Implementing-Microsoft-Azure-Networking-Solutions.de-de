---
demo:
  title: VNET-Peering (Modul 01)
  module: Module 01 - Introduction to Azure Virtual Networks
---
## Konfigurieren des VNet-Peerings

**Hinweis:** Für diese Demo benötigen Sie zwei virtuelle Netzwerke.

**Simulation:**[Verbinden zweier virtueller Netzwerke mithilfe von Azure Peering](https://mslabs.cloudguides.com/guides/AZ-700%20Lab%20Simulation%20-%20Connect%20two%20Azure%20virtual%20networks%20using%20global%20virtual%20network%20peering)

**Referenz:** [Herstellen von Verbindungen zwischen virtuellen Netzwerken mittels VNet-Peering – Tutorial](https://docs.microsoft.com/azure/virtual-network/tutorial-connect-virtual-networks-portal)

**Konfigurieren des VNet-Peerings für das erste virtuelle Netzwerk**

1. Wählen Sie im  **Azure-Portal** das erste virtuelle Netzwerk aus. Überprüfen Sie den Wert des Peerings. 

1. Wählen Sie unter  **Einstellungen** die Option  **Peerings** und **+ Neues Peering hinzufügen** aus.

1. Konfigurieren Sie das Peering des zweiten virtuellen Netzwerks. Verwenden Sie die Informationssymbole, um die verschiedenen Einstellungen zu überprüfen. 

1. Wenn das Peering abgeschlossen ist, überprüfen Sie den **Peeringstatus**. 

**Konfigurieren des VNet-Peerings für das zweite virtuelle Netzwerk**

1. Wählen Sie im  **Azure-Portal** das zweite virtuelle Netzwerk aus.

1. Wählen Sie unter  **Einstellungen** die Option  **Peerings** aus.

1. Beachten Sie, dass automatisch ein Peering erstellt wurde. Beachten Sie, dass der  **Peeringstatus**  **Verbunden** lautet.

1. Im Lab erstellen die Lernenden Peerings und testen die Verbindung zwischen virtuellen Computern. 
