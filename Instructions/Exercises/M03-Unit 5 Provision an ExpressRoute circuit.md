---
Exercise:
  title: "M03 – Lerneinheit\_5: Bereitstellen einer ExpressRoute-Verbindung"
  module: Module 03 - Design and implement Azure ExpressRoute
---
# M03 – Lerneinheit 5: Bereitstellen einer ExpressRoute-Verbindung

## Übungsszenario

In dieser Übung erstellen Sie eine ExpressRoute-Leitung mithilfe des Azure-Portals und des Azure Resource Manager-Bereitstellungsmodells.

### Geschätzte Dauer: 15 Minuten

![Diagramm: Layout der ExpressRoute-Leitung für die Übung](../media/5-exercise-provision-expressroute-circuit.png)

### Stellenqualifikationen

In dieser Übung führen Sie die folgenden Schritte aus:

+ Aufgabe 1: Erstellen und Bereitstellen einer ExpressRoute-Leitung
+ Aufgabe 2: Abrufen des Dienstschlüssels
+ Aufgabe 3: Aufheben der Bereitstellung einer ExpressRoute-Leitung


## Aufgabe 1: Erstellen und Bereitstellen einer ExpressRoute-Leitung

1. Navigieren Sie in einem Browser zum [Azure-Portal](https://portal.azure.com/) , und melden Sie sich mit Ihrem Azure-Konto an.

   >**Wichtig**: Ihre ExpressRoute-Leitung wird von dem Moment an berechnet, in dem ein Dienstschlüssel ausgegeben wird. Stellen Sie sicher, dass Sie diesen Vorgang ausführen, sobald der Konnektivitätsanbieter dazu bereit ist, die Verbindung bereitzustellen.

1. Suchen Sie im Azure-Portal den Menüeintrag **ExpressRoute-Schaltkreise**, und wählen Sie ihn aus.

1. Geben Sie auf der Seite **ExpressRoute erstellen** die **Ressourcengruppe** als `ExpressRouteResourceGroup` an. Wählen Sie dann **Standardresilienz** für **Resilienz** aus.

1. Stellen Sie sicher, dass Sie unter **Verbindungsdetails** die Region (**USA, Osten 2**), den Leitungsnamen (**TestERCircuit**), den Peeringstandort (**Seattle**), den Anbieter (**Equinix**), die Bandbreite (**50 MBit/s**), den SKU-Tarif (**Standard**) und das Abrechnungsmodell für die Datenmessung (**Getaktet**) richtig angeben.

1. Klicken Sie auf **Überprüfen + erstellen**.

1. Vergewissern Sie sich, dass die ExpressRoute-Konfiguration erfolgreich überprüft wurde, und wählen Sie dann **Erstellen** aus.

![Azure-Portal – Create ExpressRoute (ExpressRoute erstellen), Registerkarte „Konfiguration“](../media/expressroute-create-configuration2.png)

+ Der Porttyp bestimmt, ob Sie eine Verbindung mit einem Dienstanbieter oder direkt mit dem globalen Netzwerk von Microsoft an einem Peeringstandort herstellen.
+ „Neue erstellen oder aus klassischem Modell importieren“ bestimmt, ob eine neue Leitung erstellt wird oder ob eine klassische Leitung zu Azure Resource Manager migriert wird.
+ Anbieter ist der Internetdienstanbieter, von dem Sie den Dienst anfordern.
+ Der Peeringort ist der physische Standort, an dem Ihr Peering mit Microsoft stattfindet.

> [!Important]
>
> Der Peeringstandort entspricht dem [physischen Standort](https://docs.microsoft.com/en-us/azure/expressroute/expressroute-locations), an dem Ihr Peering mit Microsoft stattfindet. Dieser ist nicht mit der Eigenschaft „Standort“ verknüpft, die sich auf den geografischen Standort des Azure-Netzwerkressourcenanbieters befindet. Obgleich sie nicht miteinander in Zusammenhang stehen, sollten Sie einen Netzwerkressourcenanbieter in geografischer Nähe des Peeringstandorts der Verbindung wählen.

+ **SKU** bestimmt, ob ein ExpressRoute Local-, ExpressRoute Standard- oder ExpressRoute Premium-Add-On aktiviert wird. Sie können **Local** für die Local-SKU, **Standard** für die Standard-SKU oder **Premium** für das Premium-Add-On angeben. Sie können die SKU ändern, um das Premium-Add-On zu aktivieren.

   >**Wichtig**: Es ist nicht möglich, die SKU von Standard/Premium in Local zu ändern.

+ **Abrechnungsmodell** bestimmt den Abrechnungstyp. Sie können **Taktung** für einen Volumentarif und **Unbegrenzt** für einen Tarif mit Datenflatrate auswählen. Sie können den Abrechnungstyp von **Taktung** in **Unbegrenzt** ändern.

> [!Important]
>
> Sie können den Typ nicht von „Unbegrenzt“ in „Taktung“ ändern.

+ **Klassische Vorgänge zulassen** ermöglicht, klassische virtuelle Netzwerke mit der Verbindung zu verknüpfen.

## Aufgabe 2: Abrufen des Dienstschlüssels

1. Sie können alle Leitungen anzeigen, die Sie erstellt haben, indem Sie **Alle Dienste &gt; Netzwerk &gt; ExpressRoute-Leitungen** auswählen.

   ![Azure-Portal: Menü zum Erstellen von ExpressRoute-Ressource](../media/expressroute-circuit-menu.png)

1. Alle im Abonnement erstellten ExpressRoute-Leitungen werden hier angezeigt.

   ![Azure-Portal – Anzeigen vorhandener ExpressRoute-Leitungen](../media/expressroute-circuit-list.png)

1. Auf der Seite für die Leitung werden die Eigenschaften der Leitung angezeigt. Der Dienstschlüssel wird im Feld „Dienstschlüssel“ angezeigt. Ihr Dienstanbieter benötigt den Dienstschlüssel, um die Bereitstellung abzuschließen. Der Dienstschlüssel ist für Ihre Verbindung spezifisch. **Sie müssen den Dienstschlüssel zur Bereitstellung an Ihren Konnektivitätsanbieter senden.**

   ![Azure-Portal – Dienstschlüssel in den Eigenschaften der ExpressRoute-Leitung](../media/expressroute-circuit-overview.png)

1. Auf dieser Seite bietet der **Anbieterstatus** Informationen zum aktuellen Zustand der Bereitstellung auf der Dienstanbieterseite. **Verbindungsstatus** gibt den Zustand auf der Microsoft-Seite an.

1. Wenn Sie eine neue ExpressRoute-Verbindung erstellen, weist die Verbindung folgenden Zustand auf:

   + Anbieterstatus: Nicht bereitgestellt
   + Verbindungsstatus: Aktiviert

   + Die Verbindung wechselt in den folgenden Zustand, wenn sie vom Konnektivitätsanbieter aktuell für Sie aktiviert wird:
     + Anbieterstatus: Wird bereitgestellt
     + Verbindungsstatus: Aktiviert
   + Damit Sie eine ExpressRoute-Verbindung verwenden können, muss sie sich im folgenden Zustand befinden:
     + Anbieterstatus: Bereitgestellt
     + Schaltkreisstatus: Aktiviert
   + Sie sollten den Bereitstellungs- und Verbindungsstatus in regelmäßigen Abständen überprüfen.

![Azure-Portal – in den Eigenschaften der ExpressRoute-Leitung lautet der Status jetzt „Bereitgestellt“.](../media/provisioned.png)

Glückwunsch! Sie haben eine ExpressRoute-Leitung erstellt und den Dienstschlüssel gefunden, den Sie benötigen, um die Bereitstellung der Leitung abzuschließen.

## Aufgabe 3: Aufheben der Bereitstellung einer ExpressRoute-Leitung

Wenn der Bereitstellungsstatus des ExpressRoute-Leitungsdienstanbieters **Bereitstellung** oder **Bereitgestellt** lautet, arbeiten Sie mit Ihrem Dienstanbieter zusammen, um die Leitungsbereitstellung auf Anbieterseite aufzuheben. Microsoft kann weiterhin Ressourcen reservieren und Ihnen Kosten in Rechnung stellen, bis der Dienstanbieter die Bereitstellung der Leitung beendet und Microsoft benachrichtigt.

   >**Hinweis**: Sie müssen die Verknüpfung aller virtuellen Netzwerke mit der ExpressRoute-Leitung vor dem Aufheben der Bereitstellung aufheben. Falls dieser Vorgang nicht erfolgreich ist, überprüfen Sie, ob noch virtuelle Netzwerke mit der Verbindung verknüpft sind. Wenn der Dienstanbieter die Bereitstellung der Verbindung aufgehoben hat (Bereitstellungsstatus des Dienstanbieters lautet „Not provisioned“ (Nicht bereitgestellt)), können Sie die Verbindung löschen. Damit wird die Abrechnung für die Verbindung beendet.

## Bereinigen von Ressourcen

Sie können Ihre ExpressRoute-Verbindung löschen. Wählen Sie hierzu das Symbol **Löschen**. Sorgen Sie dafür, dass der Bereitstellungsstatus Nicht bereitgestellt lautet, bevor Sie fortfahren.

![Azure-Portal – Löschen einer ExpressRoute-Leitung](../media/expressroute-circuit-delete.png)

   >**Hinweis**: Denken Sie daran, alle neu erstellten Azure-Ressourcen zu entfernen, die Sie nicht mehr verwenden. Durch das Entfernen nicht verwendeter Ressourcen wird sichergestellt, dass keine unerwarteten Gebühren anfallen.

1. Öffnen Sie im Azure-Portal im Bereich **Cloud Shell** die **PowerShell**-Sitzung.

1. Löschen Sie alle Ressourcengruppen, die Sie während der praktischen Übungen in diesem Modul erstellt haben, indem Sie den folgenden Befehl ausführen:

   ```powershell
   Remove-AzResourceGroup -Name 'ContosoResourceGroup' -Force -AsJob
   Remove-AzResourceGroup -Name 'ExpressRouteResourceGroup' -Force -AsJob
   ```

   >**Hinweis**: Der Befehl wird (wie über den Parameter „-AsJob“ festgelegt) asynchron ausgeführt. Dies bedeutet, dass Sie zwar direkt im Anschluss einen weiteren PowerShell-Befehl in derselben PowerShell-Sitzung ausführen können, es jedoch einige Minuten dauert, bis die Ressourcengruppen tatsächlich entfernt werden.

## Erweitern Ihrer Lernerfahrung mit Copilot

Copilot kann Sie beim Erlernen der Verwendung von Azure-Skripttools unterstützen. Copilot kann Sie auch in Bereichen unterstützen, die nicht im Lab behandelt werden oder in denen Sie weitere Informationen benötigen. Öffnen Sie einen Edge-Browser, und wählen Sie „Copilot“ (rechts oben) aus, oder navigieren Sie zu *copilot.microsoft.com*. Nehmen Sie sich einige Minuten Zeit, um diese Prompts auszuprobieren.
+ Welche Serviceanbieter sind für Azure ExpressRoute verfügbar?
+ Was sind die häufigsten Konfigurationsprobleme mit Azure ExpressRoute? Was sollte ich tun, wenn ich dieses Problem habe?

## Weiterlernen im eigenen Tempo

+ [Einführung in Azure ExpressRoute](https://learn.microsoft.com/training/modules/intro-to-azure-expressroute/). In diesem Modul erfahren Sie, was Azure ExpressRoute ist und welche Funktionen es bietet.
+ [Entwerfen und implementieren Sie ExpressRoute](https://learn.microsoft.com/training/modules/design-implement-azure-expressroute/). In diesem Modul lernen Sie, wie Sie Azure ExpressRoute, ExpressRoute Global Reach und ExpressRoute FastPath entwerfen und implementieren.

## Wichtige Erkenntnisse

Herzlichen Glückwunsch zum erfolgreichen Abschluss des Labs. Hier sind die wichtigsten Erkenntnisse für dieses Lab. 
+ Azure ExpressRoute ermöglicht es Unternehmen, ihre lokalen Netzwerke direkt mit den Clouds Microsoft Azure und Microsoft 365 zu verbinden. Azure ExpressRoute verwendet eine dedizierte Verbindung mit hoher Bandbreite, die von einem Microsoft-Partner bereitgestellt wird.
+ Microsoft garantiert für dedizierte ExpressRoute-Verbindungen eine Verfügbarkeit von mindestens 99,95 %. Die Verbindung ist privat und läuft über eine Standleitung. Dritte können den Datenverkehr nicht abfangen.
+ Sie können eine Verbindung zwischen Ihrem lokalen Netzwerk und der Microsoft-Cloud auf vier verschiedene Arten erstellen: CloudExchange-Zusammenstellung, Point-to-Point-Ethernet-Verbindung, Any-to-Any-Verbindung (IPVPN) und ExpressRoute Direct.
+ Die ExpressRoute-Funktionen werden durch die SKU bestimmt: Lokal, Standard und Premuium. 


