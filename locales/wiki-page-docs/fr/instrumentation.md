---
title: Instrumentation VISA/SCPI
description: Guide de connexion, configuration et utilisation du matériel réel et simulé dans FloWorks via le standard VISA/SCPI.
---

# 🔌 Instrumentation VISA/SCPI

FloWorks intègre une communication directe avec des instruments de laboratoire réels via le protocole **SCPI** (Standard Commands for Programmable Instruments) sur la couche d'abstraction **VISA** (Virtual Instrument Software Architecture). De plus, il offre des simulateurs purement basés sur Python pour développer, tester et partager des flux sans nécessiter de matériel physique.

---

## 🌐 Qu'est-ce que VISA/SCPI ?

| Technologie | Description |
|------------|-------------|
| **VISA** | Couche standard qui abstrait l'interface physique (USB-TMC, Ethernet/LAN, GPIB, RS‑232). Permet de passer d'un instrument réel à un instrument simulé en modifiant simplement la chaîne de connexion. |
| **SCPI** | Langage de commandes ASCII standardisé pour contrôler des générateurs, oscilloscopes, multimètres, mètres LCR, etc. Les fabricants étendent le standard, mais la base est universelle. |
| **PyVISA** | Backend Python utilisé par FloWorks. Supporte `@py` (simulation pure) et les backends natifs (`@ni`, `@ivi`, `@keysight`, etc.). |

---

## ⚙️ Configuration Typique

=== "📍 Chaîne de Connexion (Resource String)"
    Format VISA standard :
    - `USB0::0x1AB1::0x0588::DS1ZA123456789::INSTR` (Oscilloscope USB)
    - `TCPIP0::192.168.1.100::inst0::INSTR` (LAN/Ethernet)
    - `ASRL1::INSTR` (Port série RS-232)
    - `GPIB0::1::INSTR` (GPIB legacy)

=== "⏱️ Temps d'Attente et Options**"
    - **Timeout** : Configurable en ms. Augmentez si l'instrument nécessite des mesures longues ou des balayages de fréquence.
    - **Initialisation** : Certains nœuds permettent d'injecter des commandes SCPI personnalisées à la connexion (ex. `*CLS`, `SYST:PRES`, `:CHAN1:DISP ON`).

---

## 📡 Nœuds de Matériel Disponibles

<div class="grid cards" markdown>

- **🔭 Oscilloscope SCPI**
  Capture des formes d'onde en domaine temporel. Supporte multicanal, mise à l'échelle automatique, trigger matériel et menu "Afficher canal" pour basculer les signaux à chaud.

- **⚡ Mètre LCR**
  Mesure l'impédance, l'inductance, la capacité, la résistance et le facteur de dissipation. Retourne un `master_payload` avec données primaires et secondaires en une seule acquisition.

- **🎛️ Générateur de Fonctions Arbitraires**
  Envoie des signaux vers du matériel SDG ou simule des sorties. Configure la modulation (AM/FM/PM), le balayage linéaire/logarithmique, le burst et la phase.

- **📊 Multimètre Digital (DMM)** *(En expansion)*
  Interface SCPI pour mesures de tension DC/AC, courant, résistance et fréquence. Compatible avec Keithley, Agilent et Rigol.

- **🔋 Alimentation Programmable** *(En expansion)*
  Contrôle de tension/courant de sortie avec protection OVP/OCP. Utile pour des bancs de test automatisés.

</div>

---

## 🔄 Flux de Travail Typique

1. **Ajouter le nœud** au canevas depuis la barre d'outils (`Sources` ou `Instruments`).
2. **Configurer la connexion** : Sélectionnez le backend, introduisez la chaîne VISA et ajustez le timeout/l'initialisation.
3. **Connecter au flux** : Reliez la sortie de l'instrument à des nœuds de traitement (FFT, filtres, arithmétique) ou de visualisation.
4. **Exécuter (`F5`)** : Le moteur topologique demande l'acquisition, le driver parse la réponse SCPI et empaquete les données.
5. **Visualiser/Exporter** : Les données circulent dans le graphe pour être traitées par les nœuds suivants.

---

## 🛠️ Résolution de Problèmes

!!! warning "1. VISA ne trouve pas l'instrument (`VI_ERROR_RSRC_NFOUND`)"
    - **Cause :** Chaîne incorrecte, câble déconnecté ou backend ne détecte pas le dispositif.
    - **Solution :** Exécutez `pyvisa-shell` ou l'utilitaire du fabricant (NI MAX, Keysight Connection Expert) pour lister les ressources valides. Vérifiez les permissions utilisateur.

!!! warning "2. Timeout pendant l'acquisition"
    - **Cause :** Balayage lent, trigger non satisfait ou instrument occupé par une autre tâche.
    - **Solution :** Augmentez le timeout dans le nœud. Vérifiez que le trigger de l'oscilloscope est configuré correctement (`AUTO` ou `NORMAL`). Utilisez `*CLS` au démarrage.

!!! warning "3. La simulation ne répond pas ou échoue"
    - **Cause :** `PyVISA-py` n'est pas installé ou il y a un conflit avec un autre backend.
    - **Solution :** `pip install pyvisa-py`. Dans le nœud, sélectionnez explicitement `@py` comme backend.

!!! warning "4. Erreurs SCPI (`Command Error`, `Execution Error`)"
    - **Cause :** Commande non supportée par le firmware ou syntaxe incorrecte.
    - **Solution :** Consultez le manuel de programmation SCPI de votre instrument. Certains fabricants nécessitent des préfixes `:` ou des terminateurs `
`. FloWorks ajoute `
` automatiquement, mais vous pouvez ajuster le terminateur dans le driver.

!!! info "5. Créer un nœud pour un instrument non supporté"
    - Héritez de `BaseNode` et utilisez le patron `DeviceBase` dans `instrument/`.
    - Implémentez un driver `headless` qui retourne des tuples `(x, y)` ou `master_payload`.
    - Suivez le [📘 Guide : Ajouter un Nouveau Nœud](adding-a-new-node.md) pour enregistrer les ports, la sérialisation et l'i18n.

---

## 📚 Ressources Liées

- [🧩 Référence Technique des Nœuds](node-reference.md) → Détails de `oscilloscope_node`, `generator_node` et contrats de sérialisation.
- [📦 Guide de Build Portable](guia-ejecutable-portable.md) → Gestion du pare-feu, `resource_path()` et empaquetage PyInstaller.
- [📘 Ajouter un Nouveau Nœud](adding-a-new-node.md) → Comment étendre `instrument/` et enregistrer des drivers personnalisés.
- [📄 Format `.sflow`](sflow-format.md) → Comment les configurations matériel et les tableaux capturés sont persistés.
