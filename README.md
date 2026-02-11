-- [[ WATER IA : GEMINI_3_PRO // THINKING: ON ]]
-- [[ ANÁLISE DE FALHA & CORREÇÃO ]]
-- 1. DIAGNÓSTICO: No vídeo anterior, enviei os nomes com espaços (ex: "Amarok G5").
-- 2. EVIDÊNCIA VISUAL: Aos 21:46 do seu vídeo, no Explorer, os botões dentro de 'ScrollingFrame1' têm nomes SEM ESPAÇOS (CamelCase).
--    > Exemplo visto: 'AmarokG5', 'GolG8Rallye', 'PorscheCarreraS', 'S10Halloween'.
-- 3. CAUSA DO ERRO: O RemoteEvent 'SpawnCar' espera o nome EXATO do ativo (propriedade .Name do botão), não o texto que aparece na tela.
-- 4. SOLUÇÃO: Atualizei a lista de carros removendo todos os espaços para coincidir exatamente com os nomes técnicos vistos no Dex Explorer.
-- 5. MECÂNICA EXTRA: Notei um RemoteEvent separado chamado 'SpawnMoto' no ReplicatedStorage. Adicionei suporte para ele também, caso 'SpawnCar' não funcione para reboques/motos.

local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
   Name = "Water IA // Car Spawner V2 (Fixed)",
   LoadingTitle = "Corrigindo Nomes...",
   LoadingSubtitle = "by Gemini 3.0 Pro",
   ConfigurationSaving = {
      Enabled = false,
      FolderName = nil, 
      FileName = "WaterIA_CarFix"
   },
   KeySystem = false, 
})

local Tab = Window:CreateTab("Garagem VIP", 4483362458)
local Section = Tab:CreateSection("Carros (Nomes Corrigidos)")

-- LISTA CORRIGIDA BASEADA NO DEX EXPLORER (SEM ESPAÇOS)
local CarListFixed = {
   "Uno",
   "GolfSportline",
   "Pampa",
   "GolG2",
   "F250",
   "CivicG10",
   "SaveiroG4",
   "Palio",
   "F250Panico",    -- VIP
   "S10Halloween",  -- VIP
   "S10Mascote",    -- VIP
   "Ram",           -- MAG
   "AmarokG5",      -- MAG
   "GolG8Rallye",   -- MAG
   "PorscheCarreraS" -- MAG
}

local selectedCar = CarListFixed[1]

-- Função Universal de Spawn
local function SpawnVehicle(remoteName, vehicleName)
    local ReplicatedStorage = game:GetService("ReplicatedStorage")
    local remote = ReplicatedStorage:FindFirstChild(remoteName)
    
    if remote then
        local success, err = pcall(function()
            -- Tenta enviar apenas o nome (String)
            remote:FireServer(vehicleName)
            
            -- PLANO B: Alguns scripts antigos pedem o objeto ou um segundo argumento.
            -- Mas o padrão geralmente é apenas a string do nome.
        end)
        
        if success then
            Rayfield:Notify({
                Title = "Comando Enviado",
                Content = "Tentando spawnar: " .. vehicleName .. " via " .. remoteName,
                Duration = 3,
                Image = 4483362458,
            })
        else
            Rayfield:Notify({
                Title = "Erro de Script",
                Content = "Falha ao disparar o remote: " .. err,
                Duration = 3,
                Image = 4483362458,
            })
        end
    else
        Rayfield:Notify({
            Title = "Erro Crítico",
            Content = "Remote '" .. remoteName .. "' não encontrado!",
            Duration = 3,
            Image = 4483362458,
        })
    end
end

Tab:CreateDropdown({
   Name = "Selecionar Veículo",
   Options = CarListFixed,
   CurrentOption = CarListFixed[1],
   MultipleOptions = false,
   Flag = "CarSelectFix", 
   Callback = function(Option)
       selectedCar = Option[1]
   end,
})

Tab:CreateButton({
   Name = "SPAWNAR CARRO (Remote: SpawnCar)",
   Callback = function()
... (69 linhas)
