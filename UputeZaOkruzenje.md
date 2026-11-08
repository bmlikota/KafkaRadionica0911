# Upute za pripremu računala za edukaciju

U nastavku su upute za pripremu računala i provjeru jesu li spremna za edukaciju.

## 1. Provjera instaliranih alata

Polaznici na svojim računalima trebaju imati instalirane sljedeće alate:

- **Java 21 ili novija verzija**
- **Docker**
  - Neki su morali napraviti sljedeće kako bi Docker proradio:
    - **Link:** [WSL 3.0.4](https://github.com/microsoft/WSL/releases/tag/3.0.4)
    - **App:** `Microsoft.WSL_3.0.4.0_x64_ARM64.msixbundle`
    - **PowerShell naredba** (potrebna su administratorska prava):

      ```powershell
      Add-AppxPackage "C:\Users\i814605\Downloads\Microsoft.WSL_3.0.4.0_x64_ARM64.msixbundle"
      ```

    - Nakon toga samo restartati Docker ako je potrebno.
- **Visual Studio Code (VS Code)** s instaliranim sljedećim ekstenzijama:
  - [Spring Boot Extension Pack](https://marketplace.visualstudio.com/items?itemName=vmware.vscode-boot-dev-pack)
  - [Markdownlint](https://marketplace.visualstudio.com/items?itemName=DavidAnson.vscode-markdownlint)
  - [Extension Pack for Java](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-pack)

**Važno:** Git Bash treba biti postavljen kao zadani terminal u VS Codeu.

`Ctrl+Shift+P` pa upisati i odabrati **Preferences: Open User Settings (JSON)** i dodati:

```json
{
   "terminal.integrated.profiles.windows": {
      "GitBash": {
         "path": "C:\\Users\\bmlikota\\Apps\\PortableGit\\bin\\bash.exe"
      }
   },
   "terminal.integrated.defaultProfile.windows": "GitBash",
   "terminal.integrated.windowsUseConptyDll": true,
}
```

## 2. Kreiranje i pokretanje Spring Boot aplikacije

1. Otvorite sljedeću poveznicu:

   [Spring Initializr – Kafka Demo](https://start.spring.io/#!type=maven-project&language=java&platformVersion=4.1.2&packaging=jar&configurationFileFormat=yaml&jvmVersion=21&groupId=hr.zaba&artifactId=kafkademo&packageName=hr.zaba.kafkademo&dependencies=web,kafka,kafka-streams,lombok)

2. Kliknite **Generate** kako biste preuzeli ZIP datoteku.

3. Raspakirajte preuzetu ZIP datoteku u željeni direktorij.

4. Otvorite terminal u raspakiranom direktoriju i izvršite sljedeću naredbu:

   ```bash
   ./mvnw clean install
   ```

   **Napomena** U mvnw trebali dodati:

   ```sh
   if [ -z "${JAVA_HOME:-}" ] || [ ! -d "${JAVA_HOME}" ] || [ "$JAVA_HOME" = "/c/Program Files/Java/jdk-11.0.14" ]; then
      if [ -d "/c/Program Files/Java/jdk-21.0.12.1" ]; then
         export JAVA_HOME="/c/Program Files/Java/jdk-21.0.12.1"
         export PATH="/c/Program Files/Java/jdk-21.0.12.1/bin:$PATH"
      fi
   fi

   if [ -f "/c/Users/h714499/.m2/wrapper/dists/apache-maven-3.9.9-bin/4nf9hui3q3djbarqar9g711ggc/apache-maven-3.9.9/bin/mvn" ]; then
      exec "/c/Users/h714499/.m2/wrapper/dists/apache-maven-3.9.9-bin/4nf9hui3q3djbarqar9g711ggc/apache-maven-3.9.9/bin/mvn" "$@"
   fi

   if [ -f "$HOME/.m2/wrapper/dists/apache-maven-3.9.9-bin/4nf9hui3q3djbarqar9g711ggc/apache-maven-3.9.9/bin/mvn" ]; then
      exec "$HOME/.m2/wrapper/dists/apache-maven-3.9.9-bin/4nf9hui3q3djbarqar9g711ggc/apache-maven-3.9.9/bin/mvn" "$@"
   fi
   ```

5. Nakon uspješnog završetka prethodne naredbe pokrenite aplikaciju na portu `8081`:

   ```bash
   ./mvnw spring-boot:run -Dspring-boot.run.arguments="--server.port=8081"
   ```

**Očekivani rezultat:** U logu se nakon pokretanja aplikacije trebaju pojavljivati samo poruke razine `INFO`.

## 3. Provjera dostupnosti aplikacije

Otvorite sljedeću adresu u pregledniku:

[http://localhost:8081](http://localhost:8081)

Ako se prikaže poruka **Whitelabel Error Page** sa statusom `404`, to je očekivano i znači da aplikacija odgovara na zahtjeve, ali nema definiranu početnu stranicu na toj adresi.

## 4. Pokretanje Docker servisa

1. Preuzmite Docker Compose datoteku iz privitka i spremite je u željeni direktorij.
2. Otvorite terminal i pozicionirajte se u direktorij u koji ste spremili datoteku.
3. Pokrenite servise sljedećom naredbom:

   ```bash
   docker compose up -d
   ```

## 5. Završna provjera

Otvorite sljedeću adresu u pregledniku:

[http://localhost:8080](http://localhost:8080)

Ako se prikaže **Kafbat UI**, servisi su pokrenuti i okruženje je spremno za edukaciju.

### Gašenje servisa

Nakon završetka provjere servise možete ugasiti sljedećom naredbom iz direktorija koji sadrži Docker Compose datoteku:

```bash
docker compose down
```
