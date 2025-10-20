---
Exercise:
  title: "M02 – Lerneinheit\_3: Erstellen und Konfigurieren eines Gateways des virtuellen Netzwerks"
  module: Module 02 - Design and implement hybrid networking
---


# M02 – Lerneinheit 3: Erstellen und Konfigurieren eines Gateways des virtuellen Netzwerks

## Übungsszenario

In dieser Übung konfigurieren Sie ein Gateway für virtuelle Netzwerke, um das Contoso Core Services-VNet und das Fertigungs-VNet zu verbinden.

![Abbildung eines VNet-Gateways](../media/3-exercise-create-configure-local-network-gateway.png)

### Stellenqualifikationen

In dieser Übung führen Sie die folgenden Schritte aus:

+ Aufgabe 1: Erstellen von CoreServicesVnet und ManufacturingVnet
+ Aufgabe 2: Erstellen von CoreServicesVM
+ Aufgabe 3: Erstellen von ManufacturingVM
+ Aufgabe 4: Herstellen einer Verbindung mit den VMs unter Verwendung von RDP
+ Aufgabe 5: Testen der Verbindung zwischen den VMs
+ Aufgabe 6: Erstellen des CoreServicesVnet-Gateways
+ Aufgabe 7: Erstellen des ManufacturingVnet-Gateways
+ Aufgabe 8: Verbinden von CoreServicesVnet mit ManufacturingVnet
+ Aufgabe 9: Verbinden von ManufacturingVnet mit CoreServicesVnet
+ Aufgabe 10: Überprüfen, ob die Verbindungen hergestellt werden
+ Aufgabe 11: Testen der Verbindung zwischen den VMs

### Geschätzte Dauer: 70 Minuten (einschließlich ca. 45 Minuten Wartezeit bei der Bereitstellung)

## Aufgabe 1: Erstellen von CoreServicesVnet und ManufacturingVnet

1. Wählen Sie im Azure-Portal das Cloud Shell-Symbol (oben rechts). Konfigurieren Sie die Shell bei Bedarf.  
    + Wählen Sie **PowerShell** aus.
    + Wählen Sie **Kein Speicherkonto erforderlich** und Ihr **Abonnement** aus und klicken Sie dann auf **Anwenden**.
    + Warten Sie, bis das Terminal erstellt wurde und eine Eingabeaufforderung angezeigt wird. 

1. Wählen Sie in der Symbolleiste des Cloud Shell-Fensters das Symbol **Dateien verwalten** aus, wählen Sie im Dropdownmenü die Option **Hochladen** und laden Sie die folgenden Dateien **azuredeploy.json** und **azuredeploy.parameters.json** in das Basisverzeichnis von Cloud Shell hoch.

        Note:: If you are working in your own subscription the [template files](https://github.com/MicrosoftLearning/AZ-700-Designing-and-Implementing-Microsoft-Azure-Networking-Solutions/tree/master/Allfiles/Exercises) are available in the GitHub lab repository.

1. Stellen Sie die folgenden ARM-Vorlagen bereit, um das virtuelle Netzwerk und die Subnetze zu erstellen, die für diese Übung erforderlich sind:

   ```powershell
   $RGName = "ContosoResourceGroup"
   #create resource group if it doesnt exist
   New-AzResourceGroup -Name $RGName -Location "eastus"
   New-AzResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile azuredeploy.json -TemplateParameterFile azuredeploy.parameters.json
   ```
   
## Aufgabe 2: Erstellen von CoreServicesVM

1. Öffnen Sie im Azure-Portal im Bereich **Cloud Shell** die **PowerShell**-Sitzung.

1. Wählen Sie in der Symbolleiste des Bereichs Cloud Shell das Symbol **Dateien verwalten**, wählen Sie im Dropdown-Menü **Hochladen** und laden Sie die folgenden Dateien **CoreServicesVMazuredeploy. json** und **CoreServicesVMazuredeploy.parameters.json** nacheinander in das Startverzeichnis der Cloud Shell aus dem Ordner **F:\Allfiles\Exercises\M02**.

1. Stellen Sie die folgenden ARM-Vorlagen bereit, um die für diese Übung erforderlichen VMs zu erstellen:

   >**Hinweis**: Sie werden aufgefordert, ein Administratorkennwort anzugeben.

   ```powershell
   $RGName = "ContosoResourceGroup"
   
   New-AzResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile CoreServicesVMazuredeploy.json -TemplateParameterFile CoreServicesVMazuredeploy.parameters.json
   ```
  
1. Wenn die Bereitstellung abgeschlossen ist, wechseln Sie zur Startseite des Azure-Portals und wählen **Virtuelle Computer** aus.

1. Überprüfen Sie, ob der virtuelle Computer erstellt wurde.

## Aufgabe 3: Erstellen von ManufacturingVM

1. Öffnen Sie im Azure-Portal im Bereich **Cloud Shell** die **PowerShell**-Sitzung.

1. Wählen Sie in der Symbolleiste des Bereichs Cloud Shell das Symbol **Dateien verwalten**, wählen Sie im Dropdown-Menü **Hochladen** und laden Sie die folgenden Dateien **ManufacturingVMazuredeploy. json** und **ManufacturingVMazuredeploy.parameters.json** nacheinander in das Startverzeichnis der Cloud Shell aus dem Ordner **F:\Allfiles\Exercises\M02**.

1. Stellen Sie die folgenden ARM-Vorlagen bereit, um die für diese Übung erforderlichen VMs zu erstellen:

   >**Hinweis**: Sie werden aufgefordert, ein Administratorkennwort anzugeben.

   ```powershell
   $RGName = "ContosoResourceGroup"
   
   New-AzResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile ManufacturingVMazuredeploy.json -TemplateParameterFile ManufacturingVMazuredeploy.parameters.json
   ```
  
1. Wenn die Bereitstellung abgeschlossen ist, wechseln Sie zur Startseite des Azure-Portals und wählen **Virtuelle Computer** aus.

1. Überprüfen Sie, ob der virtuelle Computer erstellt wurde.

## Aufgabe 4: Herstellen einer Verbindung mit den VMs unter Verwendung von RDP

1. Wählen Sie auf der Startseite des Azure-Portals die Option **Virtuelle Computer** aus.

1. Wählen Sie **ManufacturingVM** aus.

1. Wählen Sie unter **ManufacturingVM** die Option **Verbinden** und dann **RDP** aus.

1. Wählen Sie **RDP-Datei herunterladen** aus.

1. Speichern Sie die RDP-Datei auf Ihrem Desktop.

1. Stellen Sie eine Verbindung zu **ManufacturingVM** her, indem Sie die RDP-Datei, den Benutzernamen **TestUser** und das Kennwort verwenden, das Sie bei der Bereitstellung angegeben haben. Minimieren Sie nach der Verbindung die RDP-Sitzung.

1. Wählen Sie auf der Startseite des Azure-Portals die Option **Virtuelle Computer** aus.

1. Wählen Sie **CoreServicesVM** aus.

1. Wählen Sie unter **CoreServicesVM** die Option **Verbinden** und dann **RDP** aus.

1. Wählen Sie **RDP-Datei herunterladen** aus.

1. Speichern Sie die RDP-Datei auf Ihrem Desktop.

1. Stellen Sie eine Verbindung zu **CoreServicesVM** her, indem Sie die RDP-Datei, den Benutzernamen **TestUser** und das Kennwort verwenden, das Sie bei der Bereitstellung angegeben haben.

1. Wählen Sie für beide VMs unter **Wählen Sie die Datenschutzeinstellungen für Ihr Gerät aus** die Option **Akzeptieren** aus.

1. Wählen Sie für beide VMs unter **Netzwerke** die Option **Ja** aus.

1. Öffnen Sie auf **CoreServicesVM** PowerShell, und führen Sie den folgenden Befehl aus: ipconfig.

1. Notieren Sie die IPv4-Adresse.

## Aufgabe 5: Testen der Verbindung zwischen den VMs

1. Öffnen Sie auf **ManufacturingVM** PowerShell.

1. Prüfen Sie mit dem folgenden Befehl, dass keine Verbindung zu CoreServicesVM auf CoreServicesVnet besteht. Achten Sie darauf, die IPv4-Adresse für CoreServicesVM zu verwenden.

   ```Powershell
   Test-NetConnection 10.20.20.4 -port 3389
   ```

1. Die Testverbindung sollte fehlschlagen, und es wird ein ähnliches Ergebnis wie das folgende zu sehen sein:

   ![Test-NetConnection fehlgeschlagen.](../media/test-netconnection-fail.png)

## Aufgabe 6: Erstellen des CoreServicesVnet Gateway

1. Geben Sie unter **Ressourcen, Dienste und Dokumente durchsuchen (G+/)****Gateway für virtuelle Netzwerke** ein, und wählen Sie dann **Gateways für virtuelle Netzwerke** aus den Ergebnissen aus.
   ![Suchen Sie im Azure-Portal nach „Gateway für virtuelle Netzwerke“.](../media/virtual-network-gateway-search.png)

1. Wählen Sie unter Gateways für virtuelle Netzwerke die Option **+ Erstellen** aus.

1. Verwenden Sie die Informationen in der folgenden Tabelle, um das Gateway für virtuelle Netzwerke zu erstellen.

   | **TAB**         | **Bereich**       | **Option**                                  | **Wert**                    |
   | --------------- | ----------------- | ------------------------------------------- | ---------------------------- |
   | Grundlagen          | Projektdetails   | Subscription                                | Keine Änderungen erforderlich          |
   |                 |                   | ResourceGroup                               | ContosoResourceGroup         |
   |                 | Instanzdetails  | Name                                        | CoreServicesVnetGateway      |
   |                 |                   | Region                                      | USA, Osten                      |
   |                 |                   | Gatewaytyp                                | VPN                          |
   |                 |                   | SKU                                         | VpnGw1                       |
   |                 |                   | Generation                                  | Generation 1                  |
   |                 |                   | Virtuelles Netzwerk                             | CoreServicesVnet             |
   |                 |                   | Subnet                                      | GatewaySubnet (10.20.0.0/27) |
   |                 |                   | Typ der öffentlichen IP-Adresse                      | Standard                     |
   |                 | Öffentliche IP-Adresse | Öffentliche IP-Adresse                           | Neu erstellen                   |
   |                 |                   | Name der öffentlichen IP-Adresse                      | CoreServicesVnetGateway-ip   |
   |                 |                   | Aktiv/Aktiv-Modus aktivieren                   | Disabled                     |
   |                 |                   | Configure BGP (BGP konfigurieren)                               | Deaktiviert                     |
   | Überprüfen + erstellen |                   | Überprüfen Sie die Einstellungen, und wählen Sie **Erstellen** aus. |                              |

   >**Hinweis**: Die Erstellung eines Gateways für virtuelle Netzwerke kann 15 bis 30 Minuten dauern. Sie brauchen nicht zu warten, bis die Bereitstellung abgeschlossen ist. Fahren Sie mit der Erstellung des nächsten Gateways fort. 

## Aufgabe 7: Erstellen des ManufacturingVnet-Gateways

### Erstellen des GatewaySubnetzes

   >**Hinweis:** Die Vorlage hat das GatewaySubnet für das CoreServicesVnet erstellt. Hier erstellen Sie das Subnetz manuell. 

1. Suchen Sie das **ManufacturingVnet** und wählen Sie es aus.

1. Wählen Sie im Blatt **Einstellungen** die Option **Subnetze** und dann **+ Subnetz**. 

    | Parameter | value |
    | --------------- | ----------------- | 
    | Subnetzzweck | **Gateway für virtuelle Netzwerke** |
    | Größe | **/27 (32 Adressen)** |

1. Wählen Sie **Hinzufügen**. 

### Erstellen des Gateways für das lokale Netzwerk

1. Geben Sie unter **Ressourcen, Dienste und Dokumente durchsuchen (G+/)****Gateway für virtuelle Netzwerke** ein, und wählen Sie dann **Gateways für virtuelle Netzwerke** aus den Ergebnissen aus.

1. Wählen Sie unter Gateways für virtuelle Netzwerke die Option **+ Erstellen** aus.

1. Verwenden Sie diese Informationen und die Registerkarte **Einstellungen**, um das virtuelle Netzwerk-Gateway zu erstellen. 

   | **TAB**         | **Bereich**       | **Option**                                  | **Wert**                    |
   | --------------- | ----------------- | ------------------------------------------- | ---------------------------- |
   | Grundlagen          | Projektdetails   | Subscription                                | Keine Änderungen erforderlich          |
   |                 |                   | ResourceGroup                               | ContosoResourceGroup         |
   |                 | Instanzdetails  | Name                                        | ManufacturingVnetGateway     |
   |                 |                   | Region                                      | Europa, Westen                  |
   |                 |                   | Gatewaytyp                                | VPN                          |
   |                 |                   | SKU                                         | VpnGw1                       |
   |                 |                   | Generation                                  | Generation 1                  |
   |                 |                   | Virtuelles Netzwerk                             | ManufacturingVnet            |
   |                 |                   | Subnet                                      | GatewaySubnet |
   |                 |                   | Typ der öffentlichen IP-Adresse                      | Standard                     |
   |                 | Öffentliche IP-Adresse | Öffentliche IP-Adresse                           | Neu erstellen                   |
   |                 |                   | Name der öffentlichen IP-Adresse                      | ManufacturingVnetGateway-ip  |
   |                 |                   | Aktiv/Aktiv-Modus aktivieren                   | Disabled                     |
   |                 |                   | Configure BGP (BGP konfigurieren)                               | Deaktiviert                     |
   | Überprüfen + erstellen |                   | Überprüfen Sie die Einstellungen, und wählen Sie **Erstellen** aus. |                              |

   >**Hinweis**: Die Erstellung eines Gateways für virtuelle Netzwerke kann 15 bis 30 Minuten dauern.

## Aufgabe 8: Verbinden von CoreServicesVnet mit ManufacturingVnet

1. Geben Sie unter **Ressourcen, Dienste und Dokumente durchsuchen (G+/)****Gateway für virtuelle Netzwerke** ein, und wählen Sie dann **Gateways für virtuelle Netzwerke** aus den Ergebnissen aus.

1. Wählen Sie unter Gateways für virtuelle Netzwerke die Option **CoreServicesVnetGateway** aus.

1. Wählen Sie in CoreServicesGateway die Option **Verbindungen** und dann **+ Hinzufügen** aus.

   >**Hinweis**: Sie können diese Konfiguration erst abschließen, wenn die Gateways für virtuelle Netzwerke vollständig bereitgestellt wurden.

1. Verwenden Sie diese Informationen und die Registerkarte **Einstellungen**, um das virtuelle Netzwerk-Gateway zu erstellen. 


   | **Option**                     | **Wert**                         |
   | ------------------------------ | --------------------------------- |
   | Name                           | CoreServicesGW-to-ManufacturingGW |
   | Verbindungstyp                | VNet-zu-VNet                      |
   | Region                         | USA, Osten                           |
   | Erstes Gateway für virtuelle Netzwerke  | CoreServicesVnetGateway           |
   | Zweites Gateway für virtuelle Netzwerke | ManufacturingVnetGateway          |
   | Gemeinsam verwendeter Schlüssel (PSK)               | abc123                            |
   | Private Azure-IP-Adresse verwenden   | Nicht ausgewählt                      |
   | BGP aktivieren                     | Nicht ausgewählt                      |
   | IKE-Protokoll                   | IKEv2                             |
   | Subscription                   | Keine Änderungen erforderlich               |
   | Resource group                 | Keine Änderungen erforderlich               |


1. Um die Verbindung zu erstellen, wählen Sie **Überprüfen + Erstellen** und dann **Erstellen**.

## Aufgabe 9: Verbinden von ManufacturingVnet mit CoreServicesVnet

1. Geben Sie unter **Ressourcen, Dienste und Dokumente durchsuchen (G+/)****Gateway für virtuelle Netzwerke** ein, und wählen Sie dann **Gateways für virtuelle Netzwerke** aus den Ergebnissen aus.

1. Wählen Sie unter Gateways für virtuelle Netzwerke die **Option ManufacturingVnetGateway** aus.

1. Wählen Sie in CoreServicesGateway die Option **Verbindungen** und dann **+ Hinzufügen** aus.

1. Verwenden Sie die Informationen in der folgenden Tabelle zum Erstellen der Verbindung:

   | **Option**                     | **Wert**                         |
   | ------------------------------ | --------------------------------- |
   | Name                           | ManufacturingGW-to-CoreServicesGW |
   | Verbindungstyp                | VNet-zu-VNet                      |
   | Location                       | Nordeuropa                      |
   | Erstes Gateway für virtuelle Netzwerke  | ManufacturingVnetGateway          |
   | Zweites Gateway für virtuelle Netzwerke | CoreServicesVnetGateway           |
   | Gemeinsam verwendeter Schlüssel (PSK)               | abc123                            |
   | Private Azure-IP-Adresse verwenden   | Nicht ausgewählt                      |
   | BGP aktivieren                     | Nicht ausgewählt                      |
   | IKE-Protokoll                   | IKEv2                             |
   | Subscription                   | Keine Änderungen erforderlich               |
   | Resource group                 | Keine Änderungen erforderlich               |


1. Um die Verbindung zu erstellen, wählen Sie **Überprüfen + Erstellen** und dann **Erstellen**.

## Aufgabe 10: Überprüfen, ob die Verbindungen stehen

1. Geben Sie unter **Ressourcen, Dienste und Dokumente suchen (G+/)** **VPN** ein und wählen Sie dann **Verbindungen** aus den Ergebnissen aus.

1. Warten Sie, bis der Status beider Verbindungen auf **Verbunden** steht. Möglicherweise müssen Sie Ihren Bildschirm aktualisieren.

   ![VPN Gateway-Verbindungen erfolgreich erstellt.](../media/connections-status-connected.png)

## Aufgabe 11: Testen der Verbindung zwischen den VMs

1. Öffnen Sie auf **ManufacturingVM** PowerShell.

1. Prüfen Sie mit dem folgenden Befehl, dass jetzt eine Verbindung zu CoreServicesVM auf CoreServicesVnet besteht. Achten Sie darauf, die IPv4-Adresse für CoreServicesVM zu verwenden.

   ```Powershell
   Test-NetConnection 10.20.20.4 -port 3389
   ```

1. Die Testverbindung sollte erfolgreich erstellt werden, und es wird ein ähnliches Ergebnis wie das folgende zu sehen sein:

   ![Test-NetConnection erfolgreich.](../media/test-connection-succeeded.png)

1. Schließen Sie die Fenster der Remotedesktopverbindung.

## Erweitern Ihrer Lernerfahrung mit Copilot

Copilot kann Sie beim Erlernen der Verwendung von Azure-Skripttools unterstützen. Copilot kann Sie auch in Bereichen unterstützen, die nicht im Lab behandelt werden oder in denen Sie weitere Informationen benötigen. Öffnen Sie einen Edge-Browser, und wählen Sie „Copilot“ (rechts oben) aus, oder navigieren Sie zu *copilot.microsoft.com*. Nehmen Sie sich einige Minuten Zeit, um diese Prompts auszuprobieren.
+ Was sind die wichtigsten Arten von Azure VPN-Gateways und warum sollten Sie jeden Typ verwenden?
+ Welche Faktoren sollte ich bei der Auswahl der Azure VPN-Gateway-Skulptur berücksichtigen? Beispiele angeben.
+ Sind mit den Azure VPN-Gateways Kosten verbunden?


## Weiterlernen im eigenen Tempo

+ [Verbinden Sie Ihr lokales Netzwerk mit Azure über VPN Gateway](https://learn.microsoft.com/training/modules/connect-on-premises-network-with-vpn-gateway/). In diesem Modul lernen Sie, wie Sie VPN-Gateways über die CLI bereitstellen.
+ [Fehlerbehebung bei VPN-Gateways in Microsoft Azure](https://learn.microsoft.com/training/modules/troubleshoot-vpn-gateways/). In diesem Modul lernen Sie, wie Sie Site-to-Site- und Point-to-Site-VPNs überwachen und Fehler beheben können.

## Wichtige Erkenntnisse

Herzlichen Glückwunsch zum erfolgreichen Abschluss des Labs. Hier sind die wichtigsten Erkenntnisse für dieses Lab. 

+ Azure VPN Gateway ist ein Dienst, der eine sichere Verbindung zwischen Ihren lokalen Netzwerken und den virtuellen Azure-Netzwerken herstellt.
+ Site-to-Site (S2S)-Verbindungen verbinden Ihr lokales Netzwerk mit einem virtuellen Azure-Netzwerk unter Verwendung von IPsec/IKE-VPN-Tunneln. Ideal für hybride Cloud-Szenarien.
+ Point-to-Site (P2S) Verbindungen verbinden einzelne Clients von entfernten Standorten aus mit einem virtuellen Azure-Netzwerk. Zu den VPN-Protokollen gehören OpenVPN, IKEv2 oder SSTP. Nützlich für Remotearbeitskräfte.
+ VNet-to-VNet-Verbindungen verbinden zwei oder mehr virtuelle Azure-Netzwerke über IPsec/IKE-VPN-Tunnel. Geeignet für den Einsatz in mehreren Regionen oder mehreren V-Netzen.
+ Die verschiedenen VPN Gateway-SKUs bieten ein unterschiedliches Maß an Leistung, Durchsatz und Funktionen. 

