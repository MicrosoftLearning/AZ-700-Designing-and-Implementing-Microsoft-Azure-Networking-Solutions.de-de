---
Exercise:
  title: "M01 – Lerneinheit\_8: Verbinden von zwei virtuellen Azure-Netzwerken mithilfe des globalen Peerings virtueller Netzwerke"
  module: Module 01 - Introduction to Azure Virtual Networks
---

# M01 – Lerneinheit 8: Verbinden von zwei virtuellen Azure-Netzwerken mithilfe des globalen Peerings virtueller Netzwerke

## Übungsszenario

In dieser Lerneinheit konfigurieren Sie die Konnektivität zwischen „CoreServicesVnet“ und „ManufacturingVnet“, indem Sie Peerings zum Zulassen von Datenverkehr hinzufügen.

![Abbildung: Peering virtueller Netzwerke.](../media/8-exercise-connect-two-azure-virtual-networks-global.png)

### Stellenqualifikationen

In dieser Übung führen Sie die folgenden Schritte aus:

+ Aufgabe 1: Erstellen einer virtuellen Maschine zum Testen der Konfiguration
+ Aufgabe 2: Herstellen einer Verbindung mit den Test-VMs mit RDP
+ Aufgabe 3: Testen der Verbindung zwischen den VMs
+ Aufgabe 4: Erstellen von VNet-Peerings zwischen CoreServicesVnet und ManufacturingVnet
+ Aufgabe 5: Testen der Verbindung zwischen den VMs

### Interaktive Labsimulationen

>**Hinweis**: Die zuvor bereitgestellten Laborsimulationen wurden eingestellt.

### Geschätzte Dauer: 20 Minuten

## Aufgabe 1: Erstellen einer virtuellen Maschine zum Testen der Konfiguration

In diesem Abschnitt erstellen Sie eine Test-VM im VNet, um zu testen, ob Sie über Ihr VNet auf Ressourcen in einem anderen virtuellen Azure-Netzwerk zugreifen können.

### Erstellen von „ManufacturingVM“

1. Wählen Sie im Azure-Portal das Cloud Shell-Symbol (oben rechts). Konfigurieren Sie die Shell bei Bedarf.  
    + Wählen Sie **PowerShell** aus.
    + Wählen Sie **Kein Speicherkonto erforderlich** und Ihr **Abonnement** aus und klicken Sie dann auf **Anwenden**.
    + Warten Sie, bis das Terminal erstellt wurde und eine Eingabeaufforderung angezeigt wird. 

1. Wählen Sie in der Symbolleiste des Cloud Shell-Fensters das Symbol **Dateien verwalten** aus, wählen Sie im Dropdownmenü die Option **Hochladen** und laden Sie die folgenden Dateien hoch: **ManufacturingVMazuredeploy.json** und **ManufacturingVMazuredeploy.parameters.json**.

    >**Hinweis:** Wenn Sie in Ihrem eigenen Abonnement arbeiten, sind die [Vorlagendateien](https://github.com/MicrosoftLearning/AZ-700-Designing-and-Implementing-Microsoft-Azure-Networking-Solutions/tree/master/Allfiles/Exercises) im GitHub-Lab-Repository verfügbar.

1. Stellen Sie die folgenden ARM-Vorlagen bereit, um die für diese Übung erforderlichen VMs zu erstellen:

   >**Hinweis**: Sie werden aufgefordert, ein Administratorkennwort anzugeben.

   ```powershell
   $RGName = "ContosoResourceGroup"
   
   New-AzResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile ManufacturingVMazuredeploy.json -TemplateParameterFile ManufacturingVMazuredeploy.parameters.json
   ```
  
1. Wenn die Bereitstellung abgeschlossen ist, wechseln Sie zur Startseite des Azure-Portals und wählen **Virtuelle Computer** aus.

1. Überprüfen Sie, ob der virtuelle Computer erstellt wurde.

## Aufgabe 2: Herstellen einer Verbindung mit den Test-VMs mit RDP

1. Wählen Sie auf der Startseite des Azure-Portals die Option **Virtuelle Computer** aus.

1. Wählen Sie **ManufacturingVM** aus.

1. Wählen Sie unter „ManufacturingVM“ die Optionen **Verbinden &gt; RDP** aus.

1. Wählen Sie unter „ManufacturingVM \| Verbinden“ die Option **RDP-Datei herunterladen** aus.

1. Speichern Sie die RDP-Datei auf Ihrem Desktop.

1. Stellen Sie eine Verbindung zu ManufacturingVM her, indem Sie die RDP-Datei, den Benutzernamen **TestUser** und das Kennwort verwenden, das Sie während der Bereitstellung angegeben haben.

1. Wählen Sie auf der Startseite des Azure-Portals die Option **Virtuelle Computer** aus.

1. Wählen Sie **TestVM1** aus.

1. Wählen Sie unter TestVM1 **Verbinden &gt; RDP** aus.

1. Wählen Sie unter „TestVM1 \| Verbinden“ die Option **RDP-Datei herunterladen** aus.

1. Speichern Sie die RDP-Datei auf Ihrem Desktop.

1. Stellen Sie eine Verbindung zu TestVM1 her, indem Sie die RDP-Datei, den Benutzernamen **TestUser** und das Kennwort verwenden, die Sie bei der Bereitstellung angegeben haben.

1. Wählen Sie für beide VMs unter **Wählen Sie die Datenschutzeinstellungen für Ihr Gerät aus** die Option **Akzeptieren** aus.

1. Wählen Sie für beide VMs unter **Netzwerke** die Option **Ja** aus.

1. Öffnen Sie auf TestVM1 eine PowerShell-Eingabeaufforderung, und führen Sie den folgenden Befehl aus: ipconfig

1. Notieren Sie die IPv4-Adresse.

## Aufgabe 3: Testen der Verbindung zwischen den VMs

1. Öffnen Sie auf der ManufacturingVM eine PowerShell-Eingabeaufforderung.

1. Stellen Sie mithilfe des folgenden Befehls sicher, dass keine Verbindung mit TestVM1 im CoreServicesVnet besteht. Achten Sie darauf, die IPv4-Adresse für TestVM1 zu verwenden.

   ```powershell
    Test-NetConnection 10.20.20.4 -port 3389
    ```

1. Die Testverbindung sollte fehlschlagen, und es wird ein ähnliches Ergebnis wie das folgende angezeigt: ![PowerShell-Fenster mit Fehleranzeige für Befehl „Test-NetConnection 10.20.20.4 -port 3389“](../media/test-netconnection-fail.png)

## Aufgabe 4: Erstellen von VNet-Peerings zwischen CoreServicesVnet und ManufacturingVnet

1. Wählen Sie auf der Azure-Startseite **Virtuelle Netzwerke** und anschließend **CoreServicesVnet** aus.

1. Wählen Sie in „CoreServicesVnet“ unter **Einstellungen** die Option **Peerings** aus.
   ![Screenshot der Kerndienste der VNet-Peering-Einstellungen ](../media/create-peering-on-coreservicesvnet.png)

1. Wählen Sie in „CoreServicesVnet \| Peerings“ die Option **+ Hinzufügen** aus.

1. Verwenden Sie diese Informationen, um das Peering zu erstellen. Wählen Sie abschließend **Hinzufügen** aus. 

   **Zusammenfassung zu virtuellen Remotenetzwerken**

   | **Option**                                    | **Wert**                             |
   | ------------------------------------ | --------------------------------------------- | 
   | Name des Peeringlinks    | `ManufacturingVnet-to-CoreServicesVnet` |
   | Virtuelles Netzwerk | ManufacturingVnet |

    **Einstellungen für das Peering virtueller Remotenetzwerke**
   
   | **Option**                                    | **Wert**                             |
   | ------------------------------------ | --------------------------------------------- | 
   | Erlauben Sie 'ManufacturingVnet' den Zugriff auf 'CoreServicesVnet' | Aktiviert |
   |ManufacturingVnet“ soll weitergeleiteten Verkehr von ‚CoreServicesVnet‘ empfangen | Aktiviert |
 
    **Lokales virtuelles Netzwerk: Zusammenfassung**

    | **Option**                                    | **Wert**                             |
    | ------------------------------------ | --------------------------------------------- | 
    | Name des Peeringlinks | `CoreServicesVnet-to-ManufacturingVnet` |
 
    **Einstellungen für das Peering lokaler virtueller Netzwerke**
   
    | **Option**                                    | **Wert**                             |
    | ------------------------------------ | --------------------------------------------- | 
    | Erlauben Sie 'CoreServicesVnet' den Zugriff auf 'ManufacturingVnet' | Aktiviert
    | Erlauben Sie, dass 'CoreServicesVnet' weitergeleiteten Verkehr von 'ManufacturingVnet' empfängt | Aktiviert |
 
1. Überprüfen Sie in „CoreServicesVnet" \| Peerings, dass das **CoreServicesVnet-to-ManufacturingVnet** Peering **Verbunden** ist.

1. Wählen Sie unter „Virtuelle Netzwerke" die Option **ManufacturingVnet**, und überprüfen Sie, ob das Peering **ManufacturingVnet-to-CoreServicesVnet** **Verbunden** ist.

## Aufgabe 5: Testen der Verbindung zwischen den VMs

1. Öffnen Sie auf der ManufacturingVM eine PowerShell-Eingabeaufforderung.

1. Stellen Sie mithilfe des folgenden Befehls sicher, dass jetzt eine Verbindung mit TestVM1 im CoreServicesVnet besteht.

   ```powershell
    Test-NetConnection 10.20.20.4 -port 3389
    ```

1. Die Testverbindung sollte erfolgreich sein, und es wird ein ähnliches Ergebnis wie das folgende angezeigt: ![PowerShell-Fenster mit Erfolgsmeldung „TcpTestSucceeded: True“ für Befehl „Test-NetConnection 10.20.20.4 -port 3389“](../media/test-connection-succeeded.png)


## Bereinigen von Ressourcen

   >**Hinweis**: Denken Sie daran, alle neu erstellten Azure-Ressourcen zu entfernen, die Sie nicht mehr verwenden. Durch das Entfernen nicht verwendeter Ressourcen wird sichergestellt, dass keine unerwarteten Gebühren anfallen.

1. Öffnen Sie im Azure-Portal im Bereich **Cloud Shell** die **PowerShell**-Sitzung. (Erstellen Sie bei Bedarf Cloud Shell-Speicher mithilfe der Standardeinstellungen.)

1. Löschen Sie alle Ressourcengruppen, die Sie während der praktischen Übungen in diesem Modul erstellt haben, indem Sie den folgenden Befehl ausführen:

   ```powershell
   Remove-AzResourceGroup -Name 'ContosoResourceGroup' -Force -AsJob
   ```
   >**Hinweis**: Der Befehl wird (wie über den Parameter „-AsJob“ festgelegt) asynchron ausgeführt. Dies bedeutet, dass Sie zwar direkt im Anschluss einen weiteren PowerShell-Befehl in derselben PowerShell-Sitzung ausführen können, es jedoch einige Minuten dauert, bis die Ressourcengruppen tatsächlich entfernt werden.
   
## Erweitern Ihrer Lernerfahrung mit Copilot

Copilot kann Sie beim Erlernen der Verwendung von Azure-Skripttools unterstützen. Copilot kann Sie auch in Bereichen unterstützen, die nicht im Lab behandelt werden oder in denen Sie weitere Informationen benötigen. Öffnen Sie einen Edge-Browser, und wählen Sie „Copilot“ (rechts oben) aus, oder navigieren Sie zu *copilot.microsoft.com*. Nehmen Sie sich einige Minuten Zeit, um diese Prompts auszuprobieren.
+ Was sind die häufigsten Fehler bei der Konfiguration von Azure Virtual Network Peering?
+ Wenn ich in Azure Vnet1 mit Vnet2 peere und dann Vnet2 mit Vnet3, wird dann Vnet1 mit Vnet3 gepeert?
+ Können Firewalls und Gateways das Peering des virtuellen Azure-Netzwerks beeinträchtigen?


## Weiterlernen im eigenen Tempo

+ [Einführung in Azure Virtual Network](https://learn.microsoft.com/training/modules/introduction-to-azure-virtual-networks/): In diesem Modul erfahren Sie, wie Sie Azure-Netzwerkdienste entwerfen und implementieren. Sie erfahren mehr über virtuelle Netzwerke, öffentliche und private IPs, DNS, virtuelles Netzwerk-Peering, Routing und Azure Virtual NAT.
+ [Verteilen von Diensten in virtuellen Azure-Netzwerken und Integrieren dieser Dienste über Peering virtueller Netzwerke](https://learn.microsoft.com/training/modules/integrate-vnets-with-vnet-peering/): In diesem Modul lernen Sie, wie Sie virtuelles Netzwerk-Peering konfigurieren.

## Wichtige Erkenntnisse

Herzlichen Glückwunsch zum erfolgreichen Abschluss des Labs. Hier sind die wichtigsten Erkenntnisse für dieses Lab. 

+ Das Peering virtueller Netzwerke ermöglicht das nahtlose Verbinden zweier virtueller Azure-Netzwerke. Die virtuellen Netzwerke werden für Verbindungszwecke als einzelnes Element angezeigt.
+ Azure unterstützt die Verbindung von virtuellen Netzwerken innerhalb der gleichen Azure-Region und über Azure-Regionen hinweg (global).
+ Der Datenverkehr zwischen virtuellen Computern in mittels Peering verknüpften virtuellen Netzwerken wird nicht über ein Gateway oder das öffentliche Internet, sondern direkt über die Microsoft-Backbone-Infrastruktur geleitet.
+ Sie können die Größe des Adressraums der virtuellen Azure-Netzwerke mit Peering ändern, ohne dass Ausfallzeiten für den aktuellen Adressraum mit Peering auftreten.
