INSERT INTO TBCertficadosAWSLegado
(accountId,
arn,
resourceid,
CertificadoNome,
subjectAlternativeNames,
dtVencimento,
dtEmissao,
renewalEligibility,
ambiente,
InternoExterno,
NomeProduto,
Sigla,
Comunidade,
SupportGruup, -- Corrigido o nome da coluna
Status,
Observacao,
Regiao)
SELECT
RIGHT('000000000000' + CAST(@accountId AS VARCHAR(12)), 12),
@arn,
@resourceId,
@CertificadoNome,
@subjectAlternativeNames,
@dtVencimento,
@dtEmissao,
@renewalEligibility,
ambiente,
InternoExterno,
NomeProduto,
Sigla,
Comunidade,
SupportGroup, -- Corrigido o nome da coluna
Status,
Observacao,
Regiao
FROM TBFilaCertificadosAWSmanual (nolock) 
WHERE @CertificadoNome = NomeCertificado OR @arn = ArnCertificado;
