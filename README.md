# Relatório do Teste de Estágio DevOps - Pedro Henrique De Oliveira Ramos

**Para:** Equipe de Avaliação, Ivory IT
**Repositório:** [https://github.com/devpramos/ivoryit-testedevops](https://github.com/devpramos/ivoryit-testedevops)

---

## 1. Introdução

Este projeto demonstra a implementação de uma pipeline de CI/CD (Integração Contínua e Entrega Contínua) completa, conforme solicitado no teste de estágio.

O fluxo de trabalho automatiza o processo desde um `commit` local até o deploy final em nuvem, garantindo a análise de qualidade de código. As ferramentas utilizadas foram **Git**, **GitHub Actions**, **SonarCloud** e **Microsoft Azure**.

---

## 2. Link do Site Hospedado (Item 7)

O resultado final da pipeline está hospedado no Azure e pode ser acessado publicamente através do link abaixo: https://stivoryitpedro.z15.web.core.windows.net

---

## 3. Pipeline de CI/CD (Item 6)

Foi criado um workflow de GitHub Actions (`.github/workflows/main-cicd.yml`) que automatiza todo o processo de build, análise e deploy.

**Gatilho (Trigger):**
O workflow é acionado automaticamente a cada `push` ou `merge` na branch `main`.

**Etapas Principais:**
1.  **Checkout:** Baixa o código-fonte do repositório.
2.  **SonarCloud Scan:** Conecta-se ao SonarCloud usando secrets (`SONAR_TOKEN`) para realizar a Análise Estática de Código (SAST) na pasta `src/`. O "Quality Gate" é verificado, garantindo que código de baixa qualidade não seja implantado.
3.  **Deploy to Azure:** Conecta-se ao Azure usando a `AZURE_STORAGE_CONNECTION_STRING`. Em vez de usar a Ação de Static Web Apps (devido a restrições), esta etapa utiliza a **Azure CLI** (`Azure/cli@v1`) para executar o comando `az storage blob upload-batch`, enviando os arquivos de `src/` para o contêiner `$web` do site estático.

---

## 4. Fluxo de Trabalho Git (Item 5)

O projeto seguiu o fluxo de versionamento solicitado para garantir a integridade do código em produção:

1.  **Branch Temporária (ex: `feature/`):** Onde o desenvolvimento (criação do site, escrita do `.yml`) foi realizado.
2.  **`develop`:** Branch de integração. As *features* são mescladas aqui.
3.  **`homolog`:** Branch de homologação (staging). Simula o ambiente de produção. O código é promovido de `develop` para `homolog`.
4.  **`main`:** Branch de produção. Protegida e estável. Somente código validado da `homolog` é mesclado aqui, disparando o deploy.

O fluxo de promoção foi `feature` -> `develop` -> `homolog` -> `main`, utilizando **Pull Requests (PRs)** para revisão e merge em cada etapa. Evidências dos commits locais e dos PRs foram documentadas conforme solicitado.

---

## 5. Links de Avaliação

* **Projeto SonarCloud:** `https://sonarcloud.io/project/overview?id=devpramos_ivoryit-testedevops`
* **Repositório GitHub:** `https://github.com/devpramos/ivoryit-testedevops`

---

## 6. Tecnologias Utilizadas

* **Controle de Versão:** Git
* **Repositório:** GitHub
* **CI/CD:** GitHub Actions
* **Qualidade de Código:** SonarCloud (Análise Estática / SAST)
* **Nuvem (Deploy):** Azure Blob Storage (Static Website)
* **Site:** HTML5, CSS3
* **Scripting:** Azure CLI (em Bash)
