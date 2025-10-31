# Desafio DIO: Infraestrutura como Código (IaC) com AWS CloudFormation

Este repositório documenta a implementação prática de **Infraestrutura como Código (IaC)** utilizando o **AWS CloudFormation**. O objetivo foi automatizar o provisionamento e o gerenciamento do ciclo de vida de recursos na AWS por meio de *templates* declarativos.

Desenvolvido como parte dos estudos e desafios do **Bootcamp Santander Code Girls | DIO**.

---

## Conceito: 

O CloudFormation é o serviço nativo da AWS para IaC. Em vez de provisionar recursos manualmente pelo console, nós definimos a arquitetura completa em um **template** (escrito em YAML ou JSON). O serviço atua como um motor de orquestração, garantindo que o ambiente alcance o **estado final desejado** de forma consistente e rastreável.

### Vantagens da IaC

* **Consistência:** Elimina o *configuration drift*, assegurando que ambientes de Dev, Teste e Produção sejam idênticos.
* **Gestão Unificada:** Gerencia um conjunto complexo de recursos (a **Stack**) como uma única unidade.
* **Segurança e Rollback:** Garante que, em caso de falha na atualização, o ambiente retorne automaticamente ao último estado estável.
* **Versionamento:** O código da infraestrutura é armazenado e auditado no controle de versão (Git/GitHub).

---

## Implementação Prática: Provisionando um S3 Bucket

Para o laboratório, foi criado e executado um *template* CloudFormation simples, mas robusto, focado em provisionar um **S3 Bucket** com configurações de segurança que aderem às melhores práticas da AWS.

### Código do Template (`s3-bucket-template.yaml`)

O template abaixo demonstra o uso de **Funções Intrínsecas (`!Sub` e `!Ref`)** e a aplicação de políticas de segurança para bloquear acesso público:

```yaml
AWSTemplateFormatVersion: "2010-09-09"
Description: Template de S3 Bucket para o Desafio DIO com configurações de segurança.

Resources:
  DIOChallengeBucket:
    Type: AWS::S3::Bucket
    Properties:
      # Uso de !Sub para criar um nome único com ID da Conta e Região
      BucketName: !Sub "dio-cf-project-${AWS::AccountId}-${AWS::Region}"
      AccessControl: Private
      
      # Bloco obrigatório para conformidade de segurança e bloqueio de acesso público
      PublicAccessBlockConfiguration:
        BlockPublicAcls: true
        BlockPublicPolicy: true
        IgnorePublicAcls: true
        RestrictPublicBuckets: true
        
      Tags:
        - Key: Environment
          Value: Teste-DIO

Outputs:
  BucketName:
    Description: Nome final do S3 Bucket criado
    Value: !Ref DIOChallengeBucket
```
### Principais Aprendizados e Insights
- A execução deste desafio solidificou o entendimento sobre a transição do gerenciamento manual para o declarativo:
- Gestão de Stacks: Compreensão do ciclo de vida completo: criação, visualização de eventos, atualização (via Change Sets) e exclusão de recursos agrupados.
- Funções Intrínsecas: Domínio do uso de !Sub para interpolação de variáveis (como AWS::AccountId e AWS::Region) e !Ref para referenciar recursos criados.
- Segurança por Código: O insight de que a IaC não apenas automatiza, mas também impõe segurança (ex: o bloco PublicAccessBlockConfiguration) em cada deployment.
