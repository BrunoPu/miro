# Arquivo main.tf dentro do módulo "glue_schedule"

provider "aws" {
  region = "sua-regiao"
}

resource "aws_glue_trigger" "schedule" {
  name         = "seu-schedule"
  type         = "SCHEDULED"
  schedule     = "cron(30 9 ? * MON,WED *)"  # Roda toda semana às segundas e quartas às 9h30 da manhã
  workflow_name = "seu-glue-job"  # Nome do job existente

  actions {
    job_name = "seu-glue-job"
  }
}