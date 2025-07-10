# Příklady použití Azure Pipelines Agent Role

Tato složka obsahuje příklady použití Ansible role pro instalaci Azure Pipelines Agent na Ubuntu 22.04 LXC kontejnerech.

## Struktura souborů

- `basic-install.yml` - Základní playbook s výchozím nastavením
- `custom-install.yml` - Pokročilý playbook s přizpůsobenou konfigurací  
- `inventory.ini` - Příklad inventory souboru
- `README.md` - Tento soubor s instrukcemi

## Před spuštěním

1. **Upravte inventory**: Editujte `inventory.ini` a nastavte správné IP adresy a přihlašovací údaje pro vaše Ubuntu kontejnery.

2. **Ověřte připojení**: Ujistěte se, že se můžete připojit k cílovým hostům:
   ```bash
   ansible -i inventory.ini ubuntu_containers -m ping
   ```

3. **Kontrola prostředí**: Ujistěte se, že cílové hosty mají Ubuntu 22.04 a dostatečné oprávnění.

## Spuštění základního playbooku

```bash
# Spuštění základní instalace
ansible-playbook -i inventory.ini basic-install.yml

# Spuštění pouze na konkrétním hostu
ansible-playbook -i inventory.ini basic-install.yml --limit ubuntu-container-1
```

## Spuštění pokročilého playbooku

```bash
# Spuštění s přizpůsobenou konfigurací
ansible-playbook -i inventory.ini custom-install.yml

# Spuštění s dodatečnými proměnnými
ansible-playbook -i inventory.ini custom-install.yml -e "custom_image_version=20250629.3.0"
```

## Přizpůsobení role

Můžete přizpůsobit chování role pomocí těchto hlavních proměnných:

### Základní nastavení
- `image_version`: Verze obrazu (výchozí: "20250629.1.0")
- `image_os`: OS identifikátor (výchozí: "ubuntu22")
- `target_user`: Cílový uživatel pro instalaci

### Řízení instalace nástrojů
- `install_basic_packages`: Základní balíčky (true/false)
- `install_ci_cd_tools`: CI/CD nástroje (true/false)
- `install_azure_tools`: Azure nástroje (true/false)
- `install_programming_languages`: Programovací jazyky (true/false)
- `install_databases`: Databáze (true/false)
- `install_browsers`: Prohlížeče (true/false)

### Docker Hub (volitelné)
- `dockerhub_login`: Uživatelské jméno pro Docker Hub
- `dockerhub_password`: Heslo pro Docker Hub

## Řešení problémů

1. **Chyba připojení**: Zkontrolujte SSH klíče a síťové připojení
2. **Nedostatečná oprávnění**: Ujistěte se, že uživatel má sudo práva
3. **Verze Ubuntu**: Role je navržena pouze pro Ubuntu 22.04
4. **Místo na disku**: Ujistěte se, že máte dostatek volného místa (doporučuje se alespoň 20GB)

## Poznámky

- Instalace může trvat 30-60 minut v závislosti na rychlosti sítě a výkonu systému
- Role vytvoří složky v `/imagegeneration/` pro skripty a konfiguraci
- Všechny nástroje jsou nainstalovány globálně a dostupné pro všechny uživatele 