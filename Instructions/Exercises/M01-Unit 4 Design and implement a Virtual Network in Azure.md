---
Exercise:
  title: "M01 – Lerneinheit\_4: Entwerfen und Implementieren eines virtuellen Netzwerks in Azure"
  module: Module 01 - Introduction to Azure Virtual Networks
---

# M01 – Lerneinheit 4: Entwerfen und Implementieren eines virtuellen Netzwerks in Azure

## Übungsszenario

Nun können Sie virtuelle Netzwerke im Azure-Portal bereitstellen.

Betrachten wir als Beispiel das fiktive Unternehmen Contoso Ltd, das dabei ist, seine Infrastruktur und Anwendungen zu Azure zu migrieren. In Ihrer Rolle als Netzwerktechniker müssen Sie drei virtuelle Netzwerke und Subnetze planen und implementieren, um Ressourcen in diesen virtuellen Netzwerken zu unterstützen.

### Interaktive Labsimulationen

>**Hinweis**: Die zuvor bereitgestellten Laborsimulationen wurden eingestellt.

### Geschätzte Dauer: 20 Minuten

Das virtuelle Netzwerk **CoreServicesVnet** wird in der Region **USA, Osten** bereitgestellt. Dieses virtuelle Netzwerk wird über die größte Anzahl von Ressourcen verfügen. Es wird über eine VPN-Verbindung mit lokalen Netzwerken verbunden sein. Dieses Netzwerk verfügt über Webdienste, Datenbanken und andere Systeme, die für den Betrieb des Unternehmens wichtig sind. Freigegebene Dienste, wie z. B. Domänencontroller und DNS, befinden sich ebenfalls hier. Es wird ein großes Wachstum erwartet, daher ist ein großer Adressraum für dieses virtuelle Netzwerk erforderlich.

Das virtuelle Netzwerk **ManufacturingVnet** wird in der Region **Europa, Westen** in der Nähe des Standorts der Fertigungsanlagen Ihrer Organisation bereitgestellt. Dieses virtuelle Netzwerk enthält Systeme für den Betrieb der Fertigungsanlagen. Die Organisation erwartet für Ihre Systeme eine große Anzahl interner verbundener Geräte zum Abrufen von Daten wie Temperatur und benötigt einen IP-Adressraum, in den erweitert werden kann.

Das virtuelle Netzwerk **ResearchVnet** wird in der Region **Asien, Südosten** in der Nähe des Standorts des Forschungs- und Entwicklungsteams der Organisation bereitgestellt. Das Forschungs- und Entwicklungsteam verwendet dieses virtuelle Netzwerk. Das Team verfügt über einen kleinen, stabilen Satz von Ressourcen, der voraussichtlich nicht wächst. Für seine Arbeit benötigt das Team eine kleine Anzahl von IP-Adressen für einige virtuelle Computer.

![Netzwerklayout für Contoso:
Lokal 10.10.0.0/16 ResearchVNet Südostasien 10.40.40.0/24 CoreServicesVNet USA, Osten 10.20.0.0/16 ManufacturingVNet Westeuropa 10.30.0.0/16
](../media/design-implement-vnet-peering.png)

Sie erstellen die folgenden Ressourcen:

| **Virtual Network** | **Region**     | **Adressraum des virtuellen Netzwerks** | **Subnetz**                | **Subnetz**    |
| ------------------- | -------------- | --------------------------------- | ------------------------- | ------------- |
| CoreServicesVnet    | East US        | 10.20.0.0/16                      |                           |               |
|                     |                |                                   | GatewaySubnet             | 10.20.0.0/27  |
|                     |                |                                   | SharedServicesSubnet      | 10.20.10.0/24 |
|                     |                |                                   | DatabaseSubnet            | 10.20.20.0/24 |
|                     |                |                                   | PublicWebServiceSubnet    | 10.20.30.0/24 |
| ManufacturingVnet   | Europa, Westen    | 10.30.0.0/16                      |                           |               |
|                     |                |                                   | ManufacturingSystemSubnet | 10.30.10.0/24 |
|                     |                |                                   | SensorSubnet1             | 10.30.20.0/24 |
|                     |                |                                   | SensorSubnet2             | 10.30.21.0/24 |
|                     |                |                                   | SensorSubnet3             | 10.30.22.0/24 |
| ResearchVnet        | Asien, Südosten | 10.40.0.0/16                      |                           |               |
|                     |                |                                   | ResearchSystemSubnet      | 10.40.0.0/24  |

Diese virtuellen Netzwerke und Subnetze sind auf eine Weise strukturiert, die vorhandene Ressourcen unterstützt, und gleichzeitig das geplante Wachstum ermöglicht. Erstellen Sie diese virtuellen Netzwerke und Subnetze als Grundlage für Ihre Netzwerkinfrastruktur.

### Stellenqualifikationen

In dieser Übung führen Sie die folgenden Schritte aus:

+ Aufgabe 1: Erstellen der Contoso-Ressourcengruppe
+ Aufgabe 2: Erstellen des virtuellen Netzwerks „CoreServicesVnet“ und der zugehörigen Subnetze
+ Aufgabe 3: Erstellen des virtuellen Netzwerks „ManufacturingVnet“ und der zugehörigen Subnetze
+ Aufgabe 4: Erstellen des virtuellen Netzwerks „ResearchVnet“ und der zugehörigen Subnetze
+ Aufgabe 5: Überprüfen der Erstellung von VNets und Subnetzen

## Aufgabe 1: Erstellen der Contoso-Ressourcengruppe

1. Gehen Sie zum [Azure-Portal](https://portal.azure.com/).

1. Wählen Sie auf der Startseite unter **Azure-Dienste** die Option **Ressourcengruppen** aus.  

1. Wählen Sie unter „Ressourcengruppen“ die Option **+ Erstellen** aus.

1. Verwenden Sie die Informationen in der folgenden Tabelle zum Erstellen der Ressourcengruppe:

   | **Tab**         | **Option**                                 | **Wert**            |
   | --------------- | ------------------------------------------ | -------------------- |
   | Grundlagen          | Resource group                             | ContosoResourceGroup |
   |                 | Region                                     | (US) USA, Osten         |
   | Tags            | Keine Änderungen erforderlich                        |                      |
   | Bewerten + erstellen | Überprüfen Sie Ihre Einstellungen, und wählen Sie **Erstellen** aus. |                      |

1. Überprüfen Sie unter „Ressourcengruppen“, ob **ContosoResourceGroup** in der Liste angezeigt wird.

## Aufgabe 2: Erstellen des virtuellen Netzwerks „CoreServicesVnet“ und der zugehörigen Subnetze

1. Navigieren Sie auf der Homepage des Azure-Portals zur Suchleiste „Globale Suche“, suchen Sie nach **Virtuelle Netzwerke**, und wählen Sie virtuelle Netzwerke unter „Dienste“ aus.  ![Ergebnisse für „virtuelles Netzwerk“ aus der globalen Suche auf der Homepage des Azure-Portals](../media/global-search-bar.PNG)

1. Wählen Sie auf der Seite „Virtuelle Netzwerke“ die Option **Erstellen** aus.  ![Assistent zum Erstellen eines virtuellen Netzwerks](../media/create-virtual-network.png)
   
1. Verwenden Sie die Informationen in der folgenden Tabelle zum Erstellen des virtuellen Netzwerks „CoreServicesVnet“:  
   Entfernen oder überschreiben Sie den Standard-IP-Adressraum. ![IP-Adresskonfiguration für die Bereitstellung virtueller Azure-Netzwerke ](../media/default-vnet-ip-address-range-annotated.png)

   | **Tab**      | **Option**         | **Wert**            |
   | ------------ | ------------------ | -------------------- |
   | Grundlagen       | Ressourcengruppe     | ContosoResourceGroup |
   |              | Name               | CoreServicesVnet     |
   |              | Region             | (US) USA, Osten         |
   | IP-Adressen | IPv4-Adressraum | 10.20.0.0/16         |

1. Verwenden Sie die Informationen in der folgenden Tabelle zum Erstellen der Subnetze für „CoreServicesVnet“:

1. Wählen Sie **+ Subnetz hinzufügen** aus, um mit dem Erstellen der einzelnen Subnetze zu beginnen. Klicken Sie auf **Hinzufügen**, um die Erstellung der einzelnen Subnetze abzuschließen.

   | **Subnetz**             | **Option**           | **Wert**               |
   | ---------------------- | -------------------- | ----------------------- |
   | GatewaySubnet          | Subnetzzweck       | Gateway für virtuelle Netzwerke |
   |                        | Subnetzname          | GatewaySubnet           |
   |                        | Subnetzadressbereich | 10.20.0.0/27            |
   | SharedServicesSubnet   | Subnetzname          | SharedServicesSubnet    |
   |                        | Subnetzadressbereich | 10.20.10.0/24           |
   | DatabaseSubnet         | Subnetzname          | DatabaseSubnet          |
   |                        | Subnetzadressbereich | 10.20.20.0/24           |
   | PublicWebServiceSubnet | Subnetzname          | PublicWebServiceSubnet  |
   |                        | Subnetzadressbereich | 10.20.30.0/24           |

1. Um die Erstellung von „CoreServicesVnet“ und der zugehörigen Subnetze abzuschließen, wählen Sie **Überprüfen + erstellen** aus.

1. Vergewissern Sie sich, dass Ihre Konfiguration erfolgreich überprüft wurde, und klicken Sie dann auf **Erstellen**.

1. Wiederholen Sie die Schritte 1–8 für jedes VNet basierend auf den folgenden Tabellen.  

## Aufgabe 3: Erstellen des virtuellen Netzwerks „ManufacturingVnet“ und der zugehörigen Subnetze

   | **Tab**      | **Option**         | **Wert**            |
   | ------------ | ------------------ | -------------------- |
   | Grundlagen       | Ressourcengruppe     | ContosoResourceGroup |
   |              | Name               | ManufacturingVnet    |
   |              | Region             | (Europa) Europa, Westen |
   | IP-Adressen | IPv4-Adressraum | 10.30.0.0/16         |

   | **Subnetz**                | **Option**           | **Wert**                 |
   | ------------------------- | -------------------- | ------------------------- |
   | ManufacturingSystemSubnet | Subnetzname          | ManufacturingSystemSubnet |
   |                           | Subnetzadressbereich | 10.30.10.0/24             |
   | SensorSubnet1             | Subnetzname          | SensorSubnet1             |
   |                           | Subnetzadressbereich | 10.30.20.0/24             |
   | SensorSubnet2             | Subnetzname          | SensorSubnet2             |
   |                           | Subnetzadressbereich | 10.30.21.0/24             |
   | SensorSubnet3             | Subnetzname          | SensorSubnet3             |
   |                           | Subnetzadressbereich | 10.30.22.0/24             |

## Aufgabe 4: Erstellen des virtuellen Netzwerks „ResearchVnet“ und der zugehörigen Subnetze

   | **Tab**      | **Option**         | **Wert**            |
   | ------------ | ------------------ | -------------------- |
   | Grundlagen       | Ressourcengruppe     | ContosoResourceGroup |
   |              | Name               | ResearchVnet         |
   |              | Region             | Asien, Südosten       |
   | IP-Adressen | IPv4-Adressraum | 10.40.0.0/16         |

   | **Subnetz**           | **Option**           | **Wert**            |
   | -------------------- | -------------------- | -------------------- |
   | ResearchSystemSubnet | Subnetzname          | ResearchSystemSubnet |
   |                      | Subnetzadressbereich | 10.40.0.0/24         |

## Aufgabe 5: Überprüfen der Erstellung von VNets und Subnetzen

1. Wählen Sie auf der Startseite des Azure-Portals **Alle Ressourcen** aus.

1. Überprüfen Sie, ob „CoreServicesVnet“, „ManufacturingVnet“ und „ResearchVnet“ aufgeführt werden.

1. Wählen Sie **CoreServicesVnet** aus.

1. Wählen Sie in „CoreServicesVnet“ unter **Einstellungen** die Option **Subnetze** aus.

1. Überprüfen Sie unter „CoreServicesVnet \| Subnetze“, ob die von Ihnen erstellten Subnetze aufgeführt werden und die IP-Adressbereiche korrekt sind.

   ![Liste der Subnetze in „CoreServicesVnet“](../media/verify-subnets-annotated.png)

1. Wiederholen Sie die Schritte 3–5 für jedes VNet.
   
## Erweitern Ihrer Lernerfahrung mit Copilot

Copilot kann Sie beim Erlernen der Verwendung von Azure-Skripttools unterstützen. Copilot kann Sie auch in Bereichen unterstützen, die nicht im Lab behandelt werden oder in denen Sie weitere Informationen benötigen. Öffnen Sie einen Edge-Browser, und wählen Sie „Copilot“ (rechts oben) aus, oder navigieren Sie zu *copilot.microsoft.com*. Nehmen Sie sich einige Minuten Zeit, um diese Prompts auszuprobieren.
+ Können Sie ein Beispiel dafür geben, wie die IP-Adresse 10.30.0.0/16 in einem realen Szenario verwendet wird?
+ Wie lautet der Azure PowerShell-Befehl, um ein virtuelles Netzwerk namens CoreServicesVnet in der Region East (US) zu erstellen. Das virtuelle Netzwerk sollte den IP-Adressraum 10.20.0.0/16 verwenden.
+ Wie lautet der Azure CLI-Befehl, um ein virtuelles Netzwerk namens ManufacturingVnet in der Region Westeuropa zu erstellen. Das virtuelle Netzwerk sollte den IP-Adressraum 10.30.0.0/16 verwenden.

## Weiterlernen im eigenen Tempo

+ [Entwerfen Sie ein IP-Adressierungsschema für Ihren Azure-Einsatz](https://learn.microsoft.com/training/modules/design-ip-addressing-for-azure/). In diesem Modul lernen Sie die öffentlichen und privaten IP-Adressierungsmöglichkeiten von virtuellen Azure-Netzwerken kennen.
+ [Einführung in Azure Virtual Network](https://learn.microsoft.com/training/modules/introduction-to-azure-virtual-networks/): In diesem Modul erfahren Sie, wie Sie Azure-Netzwerkdienste entwerfen und implementieren. Sie erfahren mehr über virtuelle Netzwerke, öffentliche und private IPs, DNS, virtuelles Netzwerk-Peering, Routing und Azure Virtual NAT.

## Wichtige Erkenntnisse

+ Azure Virtual Network ist ein Dienst, der den Grundbaustein für Ihr privates Netzwerk in Azure bereitstellt. Eine Instanz des Diensts (ein virtuelles Netzwerk) ermöglicht zahlreichen Azure-Ressourcentypen die sichere Kommunikation untereinander sowie mit dem Internet und lokalen Netzwerken. Stellen Sie sicher, dass sich Adressräume nicht überschneiden. Stellen Sie sicher, dass der Adressraum Ihres virtuellen Netzwerks (CIDR-Block) sich nicht mit anderen Netzwerkbereichen Ihrer Organisation überschneidet.
+ Alle Azure-Ressourcen in einem virtuellen Netzwerk werden in Subnetzen innerhalb des virtuellen Netzwerks bereitgestellt. Mit Subnetzen können Sie das virtuelle Netzwerk in ein oder mehrere Subnetze unterteilen und jedem Subnetz einen Teil des Adressraums des virtuellen Netzwerks zuweisen. Die Subnetze sollten nicht den gesamten Adressraum des virtuellen Netzwerks ausmachen. Planen Sie voraus, und reservieren Sie Adressraum für die Zukunft.






