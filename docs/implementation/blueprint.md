# Blueprint: SaaS odontológico

Status: rascunho para aprovação
Data: 2026-10-16

## 1. Visão

Plataforma multi-tenant para redes odontológicas com várias unidades.
Cada rede gerencia pacientes, agenda, prontuário, orçamentos, financeiro e anexos.
O paciente não tem conta nem login. Ele interage por links seguros e por mensagens.

Projeto também serve como aprendizado de backend e frontend.
Por isso o desenvolvimento é incremental e cada decisão é documentada.

## 2. Atores

### Usuários com login

- Administrador da rede: configura unidades, profissionais, permissões e planos.
- Administrador da unidade: gerencia a operação de uma unidade. (a confirmar)
- Dentista e demais profissionais: atendem, registram prontuário, criam orçamentos.
- Recepção e assistentes: agenda, cadastro, cobrança, confirmações.

### Sem login

- Paciente: confirma consulta, preenche ficha, vê orçamento e recebe documentos
  por link seguro com token de uso limitado e expiração.

## 3. Modelo de organização (multi-tenancy)

- Tenant: a rede.
- Unidade: filial dentro da rede.
- Usuário: pertence a uma rede e pode atuar em uma ou mais unidades.
- Todo dado de negócio pertence a um tenant. Dados clínicos também pertencem a uma unidade.

A estratégia técnica de isolamento será definida em ADR próprio.

## 4. Módulos em ordem de prioridade

1. Cadastro de pacientes
2. Agenda
3. Prontuário e odontograma
4. Orçamento e plano de tratamento
5. Financeiro
6. Anexos (radiografias e fotos)
7. Lembretes por WhatsApp e/ou e-mail

Base obrigatória antes do módulo 1: autenticação, autorização, tenants, unidades, auditoria.

## 5. Fora de escopo por enquanto

- Aplicativo móvel nativo
- Portal do paciente com login
- Emissão fiscal
- Integração com convênios
- Telemedicina
- Inteligência artificial

## 6. Requisitos não funcionais

- Isolamento entre tenants verificado por testes automatizados.
- Dados de saúde são dados pessoais sensíveis (LGPD). Exigem base legal, minimização e controle de acesso.
- Auditoria de leitura e escrita em dados clínicos.
- Arquivos clínicos em armazenamento privado, nunca com URL pública permanente.
- Segredos fora do repositório.
- Contrato da API descrito em OpenAPI.
- Deploy do frontend e do backend independentes.

## 7. Decisões já tomadas

- Frontend Next.js na Vercel.
- API NestJS no Google Cloud Run.
- Worker como serviço separado no Cloud Run.
- PostgreSQL no Cloud SQL.
- Redis gerenciado, somente quando aprovado.
- Cloud Storage para arquivos privados.
- Secret Manager para segredos.
- Terraform para infraestrutura.
- Monorepo com pnpm workspaces.
- Monólito modular no backend.
- Sem Kubernetes, Kafka ou infraestrutura autogerenciada sem ADR aprovado.

## 8. Suposições a confirmar

- Escala inicial: dezenas de redes no primeiro ano.
- Primeiro usuário real: consultório da esposa do autor.
- Idioma da interface: português do Brasil.
- Região do Google Cloud: a definir (custo, latência e LGPD).

## 9. Fases

- Fase 0: planejamento e documentação (esta fase).
- Fase 1: fundação (monorepo, CI, ambiente local, auth, tenants, auditoria).
- Fase 2: pacientes e agenda.
- Fase 3: prontuário e odontograma.
- Fase 4: orçamento e financeiro.
- Fase 5: anexos e lembretes.

## 10. Decisões que exigem aprovação humana

- Região e custos do Google Cloud.
- Provedor de Redis gerenciado.
- Provedor de WhatsApp (API oficial via parceiro) e de e-mail transacional.
- Provedor de autenticação, se não for implementada internamente.
- Plano da Vercel.
