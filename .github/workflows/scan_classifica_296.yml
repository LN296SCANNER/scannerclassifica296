name: Scan Classifica 296

on:
  workflow_dispatch:
  schedule:
    - cron: '0 0 */3 * *' # Ogni 3 giorni a mezzanotte (aggiusta come preferisci)

jobs:
  scan_and_push:
    runs-on: ubuntu-latest
    
    steps:
      - name: Checkout Code
        uses: actions/checkout@v4

      - name: Setup Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.9'

      - name: Install Dependencies
        run: |
          pip install playwright requests
          playwright install chromium --with-deps

      # 1. ESEGUI LO SCANNER 296
      - name: Run 296 Scanner
        env:
          LK_EMAIL: ${{ secrets.LK_EMAIL }}
          LK_PASSWORD: ${{ secrets.LK_PASSWORD }}
          TELEGRAM_TOKEN: ${{ secrets.TELEGRAM_TOKEN }}
          TELEGRAM_CHAT_ID: ${{ secrets.TELEGRAM_CHAT_ID }}
        # Assicurati di lanciare lo script corretto
        run: python main_296.py

      # 2. PUSH SU RE-PANZA/LK_DATABASE
      - name: Push to Re-Panza/lk_database
        env:
          MY_PAT: ${{ secrets.PAT_GITHUB }}
          TARGET_REPO: "Re-Panza/lk_database"
          FILE_NAME: "database_classificamondo296.json"
        run: |
          # Clona la repo di destinazione
          git clone https://x-access-token:${MY_PAT}@github.com/${{ env.TARGET_REPO }}.git db_dest
          
          # Sposta il file generato
          mv ${{ env.FILE_NAME }} db_dest/
          
          # Entra, committa e pusha
          cd db_dest
          git config --global user.name "Re Panza Bot"
          git config --global user.email "bot@repanza.it"
          
          git add ${{ env.FILE_NAME }}
          
          # Push solo se ci sono cambiamenti
          git diff --quiet && git diff --staged --quiet || (git commit -m "📊 Classifica 296 Update: $(date)" && git push)
          
      - name: Upload Debug Artifacts (in case of failure)
        if: failure()
        uses: actions/upload-artifact@v4
        with:
          name: debug-screenshots-296
          path: debug_*.png
