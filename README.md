# MSWindows11

# Remover Agrupamento por Data no Windows Explorer

Este repositório contém dois scripts em **PowerShell** para desativar o agrupamento por data no Windows Explorer.

---

## ⚠️ Aviso Importante

Estes scripts **alteram configurações de exibição** do Windows Explorer e podem **remover personalizações anteriores**. Use com cautela e esteja ciente das consequências.

---

## 🧩 Script 1: Remover agrupamento apenas na pasta "Downloads"

Este script ajusta as configurações de visualização especificamente para a pasta "Downloads", definindo-a como "Detalhes" e desativando qualquer agrupamento.

```powershell
# Caminhos base no Registro do Windows
$bagsPath = "HKCU:\SOFTWARE\Classes\Local Settings\Software\Microsoft\Windows\Shell\Bags"
$bagsKey = "$bagsPath\AllFolders\Shell"

# Garante que a chave exista
If (!(Test-Path $bagsKey)) {
    New-Item -Path $bagsKey -Force | Out-Null
}

# Define a visualização como Detalhes (4) e desativa agrupamento
New-ItemProperty -Path $bagsKey -Name "LogicalViewMode" -Value 4 -PropertyType DWord -Force | Out-Null
New-ItemProperty -Path $bagsKey -Name "Mode" -Value 4 -PropertyType DWord -Force | Out-Null

# Remove personalizações antigas das pastas
Remove-Item "$bagsPath\*" -Recurse -Force -ErrorAction SilentlyContinue

# Reinicia o Explorer para aplicar
Stop-Process -Name explorer -Force
Start-Process explorer
```
---
## 🧩 Script 2: Aplicar a configuração para todas as pastas

Este script limpa todas as configurações de visualização existentes e aplica uma configuração padrão de "Detalhes" sem agrupamento para todas as pastas do Windows Explorer.

```powershell
# Caminho para personalizações de exibição
$bagsPath = "HKCU:\SOFTWARE\Classes\Local Settings\Software\Microsoft\Windows\Shell\Bags"

# Limpa todas as configurações antigas
Remove-Item "$bagsPath" -Recurse -Force -ErrorAction SilentlyContinue

# Cria chave padrão com visualização em Detalhes sem agrupamento
New-Item -Path "$bagsPath\AllFolders\Shell" -Force | Out-Null
New-ItemProperty -Path "$bagsPath\AllFolders\Shell" -Name "LogicalViewMode" -Value 4 -PropertyType DWord -Force | Out-Null
New-ItemProperty -Path "$bagsPath\AllFolders\Shell" -Name "Mode" -Value 4 -PropertyType DWord -Force | Out-Null
New-ItemProperty -Path "$bagsPath\AllFolders\Shell" -Name "Flags" -Value 0 -PropertyType DWord -Force | Out-Null

# Reinicia o Explorer para aplicar as alterações
Stop-Process -Name explorer -Force
Start-Process explorer
```

---
## ✅ Como usar

Abra o PowerShell como Administrador.
Copie o conteúdo do script desejado.
Cole no terminal do PowerShell.
Pressione Enter.
O Windows Explorer será reiniciado automaticamente com o agrupamento desativado de acordo com o script escolhido.



