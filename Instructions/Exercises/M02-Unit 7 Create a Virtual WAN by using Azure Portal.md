---
Exercise:
  title: "M02 – Lerneinheit\_7: Erstellen eines Virtual WAN mithilfe des Azure-Portals"
  module: Module 02 - Design and implement hybrid networking
---

# M02 – Lerneinheit 7: Erstellen eines Virtual WAN mithilfe des Azure-Portals

## Übungsszenario

In dieser Übung erstellen Sie ein Virtual WAN für Contoso.

![Diagramm der WAN-Architektur des virtuellen Netzwerks.](../media/7-exercise-create-virtual-wan-by-using-azure-portal.png)

### Stellenqualifikationen
In dieser Übung führen Sie die folgenden Schritte aus:

+ Aufgabe 1: Erstellen eines virtuellen WAN
+ Aufgabe 2: Erstellen eines Hubs mithilfe des Azure-Portals
+ Aufgabe 3: Verbinden eines VNet mit dem virtuellen Hub

### Interaktive Labsimulationen

>**Hinweis**: Die zuvor bereitgestellten Laborsimulationen wurden eingestellt.
>
### Geschätzte Dauer: 65 Minuten (einschließlich ca. 45 Minuten Wartezeit bei der Bereitstellung)

## Aufgabe 1: Erstellen eines virtuellen WAN

1. Navigieren Sie in einem Browser zum Azure-Portal, und melden Sie sich mit Ihrem Azure-Konto an.

1. Geben Sie im Portal „Virtual WAN“ in das Suchfeld ein, und wählen Sie in der Ergebnisliste **Virtuelle WANs** aus.

   ![Suchen nach Virtual WAN im Azure-Portal.](../media/search-for-virtual-wan.png)

1. Klicken Sie auf der Seite „Virtual WAN“ auf **+ Erstellen**.

1. Füllen Sie auf der Seite WAN erstellen auf der Registerkarte **Grundlagen** die folgenden Felder aus:

   + **Abonnement:** Verwenden Sie das vorhandene Abonnement.

   + **Ressourcengruppe:** ContosoResourceGroup

   + **Ressourcengruppenstandort:** Wählen Sie in der Dropdownliste einen Ressourcenstandort aus. Ein WAN ist eine globale Ressource, die nicht in einer bestimmten Region angeordnet ist. Sie müssen aber eine Region auswählen, damit Sie die von Ihnen erstellte WAN-Ressource verwalten und finden können.

   + **Name:** ContosoVirtualWAN

   + **Typ:** Standard

1. Wenn Sie die Felder ausgefüllt haben, wählen Sie **Überprüfen + Erstellen** aus.

1. Klicken Sie nach bestandenen Überprüfung auf **Erstellen**, um das virtuelle WAN zu erstellen.

## Aufgabe 2: Erstellen eines Hubs mithilfe des Azure-Portals

Ein Hub enthält Gateways für Verbindungen vom Typ „Site-to-Site“, „ExpressRoute“ oder „Point-to-Site“. Es dauert 30 Minuten, um das Site-to-Site-VPN-Gateway im virtuellen Hub zu erstellen. Sie müssen ein Virtual WAN erstellen, bevor Sie einen Hub erstellen können.

1. Suchen Sie das virtuelle WAN, das Sie erstellt haben.
   
1. Klicken Sie auf der Seite Virtual WAN unter **Konnektivität** auf **Hubs**.

1. Wählen Sie auf der Seite Hubs **+ Neuer Hub** aus, um die Seite „Virtuellen Hub erstellen“ zu öffnen.
   ![Erstellen eines virtuellen Hubs, Registerkarte „Grundlagen“.](../media/create-vwan-hub.png)

1. Füllen Sie auf der Seite Virtuellen Hub erstellen auf der Registerkarte **Grundlagen** die folgenden Felder aus:
   + **Region:** USA, Westen
   + **Name:** ContosoVirtualWANHub-WestUS
   + **Privater Adressraum des Hubs:** 10.60.0.0/24
   + **Virtuelle Hubkapazität:** 2 Routinginfrastruktureinheiten
   + **Hubroutingpräferenz.** Übernehmen Sie die Standardeinstellung

1. Klicken Sie auf **Weiter: Site-to-Site**.

1. Füllen Sie auf der Registerkarte **Site-to-Site** die folgenden Felder aus:
   + **Möchten Sie eine Site-to-Site-Verbindung (VPN-Gateway) erstellen?** Ja
   + Das Feld **AS-Nummer** kann nicht bearbeitet werden.
   + **Gatewayskalierungseinheiten:** 1 Skalierungseinheit = 500 MBit/s x 2
   + **Routingpräferenz:** Übernehmen Sie die Standardeinstellung

1. Wählen Sie zum Überprüfen **Überprüfen + erstellen** aus.

1. Wählen Sie **Erstellen** aus, um den Hub zu erstellen.

1. Klicken Sie nach 30 Minuten auf **Aktualisieren**, um den Hub auf der Seite Hubs anzuzeigen.

## Aufgabe 3: Verbinden eines VNet mit dem virtuellen Hub

1. Suchen Sie das virtuelle WAN, das Sie erstellt haben.

1. Wählen Sie in ContosoVirtualWAN unter **Konnektivität** die Option **Virtuelle Netzwerkverbindungen** aus.

   ![Virtual WAN-Konfigurationsseite mit hervorgehobenen virtuellen Netzwerkverbindungen.](../media/connect-vnet-to-virtual-hub.png)

1. Wählen Sie unter ContosoVirtualWAN | Virtuelle Netzwerkverbindungen **+ Verbindung hinzufügen** aus.

1. Verwenden Sie unter Verbindung hinzufügen die folgenden Informationen, um die Verbindung zu erstellen.

   + **Verbindungsname:** ContosoVirtualWAN-to-ResearchVNet

   + **Hubs:** ContosoVirtualWANHub-WestUS

   + **Abonnement:** keine Änderungen

   + **Ressourcengruppe:** ContosoResourceGroup

   + **Virtuelles Netzwerk:** ResearchVNet

   + **An keine verteilen:** Ja

   + **Routingtabelle zuordnen:** Standard

1. Klicken Sie auf **Erstellen**.

## Bereinigen von Ressourcen

   >**Hinweis**: Denken Sie daran, alle neu erstellten Azure-Ressourcen zu entfernen, die Sie nicht mehr verwenden. Durch das Entfernen nicht verwendeter Ressourcen wird sichergestellt, dass keine unerwarteten Gebühren anfallen.

1. Öffnen Sie im Azure-Portal im Bereich **Cloud Shell** die **PowerShell**-Sitzung.

1. Löschen Sie alle Ressourcengruppen, die Sie während der praktischen Übungen in diesem Modul erstellt haben, indem Sie den folgenden Befehl ausführen:

   ```powershell
   Remove-AzResourceGroup -Name 'ContosoResourceGroup' -Force -AsJob
   ```

   >**Hinweis**: Der Befehl wird (wie über den Parameter „-AsJob“ festgelegt) asynchron ausgeführt. Dies bedeutet, dass Sie zwar direkt im Anschluss einen weiteren PowerShell-Befehl in derselben PowerShell-Sitzung ausführen können, es jedoch einige Minuten dauert, bis die Ressourcengruppen tatsächlich entfernt werden.

## Erweitern Ihrer Lernerfahrung mit Copilot

Copilot kann Sie beim Erlernen der Verwendung von Azure-Skripttools unterstützen. Copilot kann Sie auch in Bereichen unterstützen, die nicht im Lab behandelt werden oder in denen Sie weitere Informationen benötigen. Öffnen Sie einen Edge-Browser, und wählen Sie „Copilot“ (rechts oben) aus, oder navigieren Sie zu *copilot.microsoft.com*. Nehmen Sie sich einige Minuten Zeit, um diese Prompts auszuprobieren.
+ Welche Art von Netzwerkarchitektur verwendet Azure VWAN?
+ Was sind die Unterschiede zwischen Azure VWAN Basic und Standard? Stellen Sie Beispiele bereit.
+ Kann ein Azure VWAN mit Skripting-Tools erstellt werden?

## Weiterlernen im eigenen Tempo

+ [Einführung in Azure Virtual WAN](https://learn.microsoft.com/training/modules/introduction-azure-virtual-wan/). In diesem Modul lernen Sie die Funktionen und Merkmale von Azure Virtual WAN kennen. 
+ [Entwerfen und implementieren Sie hybride Netzwerke](https://learn.microsoft.com/training/modules/design-implement-hybrid-networking/). In diesem Modul lernen Sie, wie Sie Azure Virtual WAN entwerfen und implementieren.

## Wichtige Erkenntnisse

Herzlichen Glückwunsch zum erfolgreichen Abschluss des Labs. Hier sind die wichtigsten Erkenntnisse für dieses Lab. 

+ Azure Virtual WAN ist ein Netzwerkdienst, der viele Netzwerk-, Sicherheits- und Routingfunktionen auf einer einzigen Bedienoberfläche vereint
+ Die virtuelle WAN-Architektur ist eine Hub-and-Spoke-Architektur mit integrierter Skalierung und Leistung für Zweigstellen, Benutzende, ExpressRoute-Schaltungen und virtuelle Netzwerke.
+ Es gibt drei Hauptanwendungsfälle für virtuelles WAN: Standort zu Standort, Punkt zu Standort und ExpressRoute. 
+ Es gibt zwei Arten von virtuellen WANs: Basic (nur Site-to-Site VPN) und Standard.









