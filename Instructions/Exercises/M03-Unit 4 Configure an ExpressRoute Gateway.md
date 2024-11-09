---
Exercise:
  title: "M03 – Lerneinheit\_4: Konfigurieren eines ExpressRoute-Gateways"
  module: Module 03 - Design and implement Azure ExpressRoute
---
# M03 – Lerneinheit 4: Konfigurieren eines ExpressRoute-Gateways

## Übungsszenario

![Abbildung eines VNet-Gateways](../media/4-exercise-configure-expressroute-gateway.png)

Wenn Sie Ihr virtuelles Azure-Netzwerk und Ihr lokales Netzwerk über ExpressRoute verbinden möchten, müssen Sie ein virtuelles Netzwerkgateway erstellen. Ein Gateway für virtuelle Netzwerke dient zwei Zwecken: dem Austausch von IP-Routen zwischen den Netzwerken und der Weiterleitung des Netzwerkdatenverkehrs.

**Hinweis:** Eine **[interaktive Labsimulation](https://mslabs.cloudguides.com/guides/AZ-700%20Lab%20Simulation%20-%20Configure%20an%20ExpressRoute%20gateway)** ist verfügbar, mit der Sie dieses Lab in Ihrem eigenen Tempo durcharbeiten können. Möglicherweise liegen geringfügige Unterschiede zwischen der interaktiven Simulation und dem gehosteten Lab vor, aber die dargestellten Kernkonzepte und Ideen sind identisch.

### Geschätzte Dauer: 60 Minuten (einschließlich ca. 45 Minuten Wartezeit für die Bereitstellung)

**Gatewaytypen**

Beim Erstellen eines Gateways für virtuelle Netzwerke müssen mehrere Einstellungen angegeben werden. Eine dieser erforderlichen Einstellungen, „-GatewayType“, gibt an, ob das Gateway für ExpressRoute- oder für VPN-Datenverkehr verwendet wird. Die zwei Gatewaytypen sind:

- **VPN**: Wenn verschlüsselter Netzwerkdatenverkehr über das öffentliche Internet gesendet wird, verwenden Sie den Gatewaytyp „VPN“. Dies wird auch als VPN Gateway bezeichnet. Bei Site-to-Site-, Point-to-Site- und VNet-zu-VNet-Verbindungen wird jeweils VPN Gateway verwendet.
- **ExpressRoute** – Verwenden Sie den Gatewaytyp „ExpressRoute“, wenn Netzwerkdatenverkehr über eine private Verbindung gesendet wird. Dies wird auch als ExpressRoute-Gateway bezeichnet und ist der Gatewaytyp, der auch zum Konfigurieren von ExpressRoute verwendet wird.

Ein virtuelles Netzwerk kann pro Gatewaytyp immer nur über ein einzelnes virtuelles Netzwerkgateway verfügen. So können Sie beispielsweise ein virtuelles Netzwerkgateway mit „-GatewayType VPN“ und ein virtuelles Netzwerkgateway mit „-GatewayType ExpressRoute“ verwenden.

In dieser Übung führen Sie die folgenden Schritte aus:

- Aufgabe 1: Erstellen des VNet und des Gatewaysubnetzes
- Aufgabe 2: Erstellen des virtuellen Netzwerkgateways

## Aufgabe 1: Erstellen des VNet und des Gatewaysubnetzes

1. Geben Sie auf einer beliebigen Seite im Azure-Portal unter **Ressourcen, Dienste und Dokumente durchsuchen** den Begriff „Virtuelles Netzwerk“ ein, und wählen Sie dann **Virtuelle Netzwerke** aus den Ergebnissen aus.

1. Wählen Sie auf der Seite „Virtuelle Netzwerke“ die Option **+ Erstellen** aus.

1. Verwenden Sie im Bereich „Virtuelles Netzwerk erstellen“ auf der Registerkarte **Grundlagen** die Informationen in der folgenden Tabelle, um das VNet zu erstellen:

   | **Einstellung**          | **Wert**                        |
   | -------------------- | -------------------------------- |
   | Name des virtuellen Netzwerks | CoreServicesVNet                 |
   | Ressourcengruppe       | ContosoResourceGroup             |
   | Standort             | East US                          |

1. Klicken Sie auf **Weiter: IP-Adressen**.

1. Geben Sie auf der Registerkarte **IP-Adressen** in **IPv4-Adressraum** „10.20.0.0/16“ ein, und wählen Sie dann **+ Subnetz hinzufügen** aus.

1. Verwenden Sie im Bereich „Subnetz hinzufügen“ die Informationen in der folgenden Tabelle, um das Subnetz zu erstellen:

   | **Einstellung**                  | **Wert**     |
   | ---------------------------- | ------------- |
   | Name des Gatewaysubnetzes          | GatewaySubnet |
   | Adressraum des Gatewaysubnetzes | 10.20.0.0/27  |

1. Wählen Sie dann **Hinzufügen** aus.

1. Wählen Sie auf der Seite „Virtuelles Netzwerk erstellen“ die Option **Überprüfen und erstellen** aus.

   ![Azure-Portal – Gatewaysubnetz hinzufügen](../media/add-gateway-subnet.png)

1. Vergewissern Sie sich, dass das VNet erfolgreich überprüft wurde, und wählen Sie dann **Erstellen** aus.

>**Hinweis:** Wenn Sie ein virtuelles Dual-Stack-Netzwerk verwenden und planen, IPv6-basiertes privates Peering über ExpressRoute zu nutzen, wählen Sie „IP6-Adressraum hinzufügen“ aus und geben Sie Werte für den IPv6-Adressbereich ein.

## Aufgabe 2: Erstellen des virtuellen Netzwerkgateways

1. Geben Sie auf einer beliebigen Seite im Azure-Portal unter **Ressourcen, Dienste und Dokumente durchsuchen (G+/)** das Gateway für virtuelle Netzwerke ein, und wählen Sie dann **Gateways für virtuelle Netzwerke** aus den Ergebnissen aus.

1. Wählen Sie auf der Seite „Gateways für virtuelle Netzwerke“ die Option **+ Erstellen** aus.

1. Verwenden Sie auf der Seite **Gateway für virtuelle Netzwerke erstellen** die Informationen in der folgenden Tabelle, um das Gateway zu erstellen:

   | **Einstellung**               | **Wert**                  |
   | ------------------------- | -------------------------- |
   | **Projektdetails**       |                            |
   | Ressourcengruppe            | ContosoResourceGroup       |
   | **Instanzendetails**      |                            |
   | Name                      | CoreServicesVnetGateway    |
   | Region                    | USA, Osten                    |
   | Gatewaytyp              | ExpressRoute               |
   | SKU                       | Standard                   |
   | Virtuelles Netzwerk           | CoreServicesVNet           |
   | **Öffentliche IP-Adresse**     |                            |
   | Öffentliche IP-Adresse         | Neu erstellen                 |
   | Name der öffentlichen IP-Adresse    | CoreServicesVnetGateway-IP |
   | Zuweisung                | Nicht konfigurierbar           |

1. Klicken Sie auf **Überprüfen + erstellen**.

1. Vergewissern Sie sich, dass die Gatewaykonfiguration erfolgreich überprüft wurde, und wählen Sie dann **Erstellen** aus.

1. Wählen Sie nach Abschluss der Bereitstellung die Option **Zu Ressourcengruppe wechseln** aus.

>**Hinweis:** Die Bereitstellung eines Gateways kann bis zu 45 Minuten dauern.


## Erweitern Ihrer Lernerfahrung mit Copilot

Copilot kann Sie beim Erlernen der Verwendung von Azure-Skripttools unterstützen. Copilot kann Sie auch in Bereichen unterstützen, die nicht im Lab behandelt werden oder in denen Sie weitere Informationen benötigen. Öffnen Sie einen Edge-Browser, und wählen Sie „Copilot“ (rechts oben) aus, oder navigieren Sie zu *copilot.microsoft.com*. Nehmen Sie sich einige Minuten Zeit, um diese Prompts auszuprobieren.
+ Wie unterscheidet sich Azure ExpressRoute von Virtual WAN? Können Sie die Technologien gemeinsam nutzen? Bereitstellen von Beispielen.
+ Was sollte ich bei der Wahl zwischen einem ExpressRoute-Anbietermodell und ExpressRoute Direct beachten?
+ Erstellen Sie eine Tabelle, die die Azure ExpressRoute SKU und ihre Funktionen zusammenfasst.

## Weiterlernen im eigenen Tempo

+ [Einführung in Azure ExpressRoute](https://learn.microsoft.com/training/modules/intro-to-azure-expressroute/). In diesem Modul erfahren Sie, was Azure ExpressRoute ist und welche Funktionen es bietet.
+ [Entwerfen und implementieren Sie ExpressRoute](https://learn.microsoft.com/training/modules/design-implement-azure-expressroute/). In diesem Modul lernen Sie, wie Sie Azure ExpressRoute, ExpressRoute Global Reach und ExpressRoute FastPath entwerfen und implementieren.

## Wichtige Erkenntnisse

Herzlichen Glückwunsch zum erfolgreichen Abschluss des Labs. Hier sind die wichtigsten Erkenntnisse für dieses Lab. 
+ Azure ExpressRoute ermöglicht es Unternehmen, ihre lokalen Netzwerke direkt mit den Clouds Microsoft Azure und Microsoft 365 zu verbinden. Azure ExpressRoute verwendet eine dedizierte Verbindung mit hoher Bandbreite, die von einem Microsoft-Partner bereitgestellt wird.
+ Microsoft garantiert für dedizierte ExpressRoute-Verbindungen eine Verfügbarkeit von mindestens 99,95 %. Die Verbindung ist privat und läuft über eine Standleitung. Dritte können den Datenverkehr nicht abfangen.
+ Sie können eine Verbindung zwischen Ihrem lokalen Netzwerk und der Microsoft-Cloud auf vier verschiedene Arten erstellen: CloudExchange-Zusammenstellung, Point-to-Point-Ethernet-Verbindung, Any-to-Any-Verbindung (IPVPN) und ExpressRoute Direct.
+ Die ExpressRoute-Funktionen werden durch die SKU bestimmt: Lokal, Standard und Premuium. 
