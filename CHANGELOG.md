# Changelog

## [Unreleased] - 2025-01-XX

### Added
- Initial release of VMForge

### Notes
- **Amélioration recommandée** : Utiliser les images cloud des distributions au lieu de virt-builder pour bénéficier du format qcow2 (croissance dynamique du disque). Les images cloud offrent une meilleure fiabilité de boot et une gestion optimisée de l'espace disque.

## [v0.0.1] - 2025-01-XX

### Added
- Interactive and CLI modes for VM creation
- Support for Debian and Ubuntu via virt-builder
- Automatic SSH key generation and injection
- Custom package installation
- Network configuration with DHCP
- Essential packages auto-installed (openssh-server, sudo)
