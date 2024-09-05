--modificado
SELECT 
    P.CD_PACIENTE,
    P.NM_PACIENTE AS NOME,
    P.TP_SEXO AS SEXO,
    TRUNC((A.DT_ATENDIMENTO - P.DT_NASCIMENTO) / 365) AS IDADE,
    TO_CHAR(A.DT_ATENDIMENTO, 'DD/MM/YYYY') AS DATA_INTERNACAO,
    TO_CHAR(A.DT_ALTA, 'DD/MM/YYYY') AS DATA_ALTA,
    MA.DS_MOT_ALT AS MOTIVO_ALTA,
    c.DS_CID AS CID,
    re.NM_CAMPO AS EXAME,
    re.DS_RESULTADO AS RESULTADO,
    pl.DT_EXAME AS DATA_EXAME,
    av.DT_AVISO_CIRURGIA AS DATA_AVISO_CIRURGIA,
    ci.DS_CIRURGIA AS CIRURGIA,

    CASE 
        WHEN av.CD_ASA = 'ASA I' THEN ' (Paciente saudável)'
        WHEN av.CD_ASA = 'ASA II' THEN 'ASA II (Paciente com enfermidade sistêmica leve)'
        WHEN av.CD_ASA = 'ASA III' THEN 'ASA III (Paciente com enfermidade sistêmica severa)'
        WHEN av.CD_ASA = 'ASA IV' THEN 'ASA IV (Paciente com enfermidade sistêmica e com risco de vida)'
        WHEN av.CD_ASA = 'ASA V' THEN 'ASA V (Paciente com expectativa de óbito antes de 24 horas)'
        WHEN av.CD_ASA = 'E' THEN 'Emergência'
        ELSE 'Desconhecido'
    END AS CLASSIFICACAO_ASA,

    CASE 
        WHEN av.tp_cirurgias = 'E' THEN 'Cirurgia Eletiva'
        WHEN av.tp_cirurgias = 'M' THEN 'Emergência'
        WHEN av.tp_cirurgias = 'U' THEN 'Urgência'
        ELSE 'Desconhecido'
    END AS TIPO_CIRURGIA,

    CASE 
        WHEN av.sn_utiliza_sangue = 'S' THEN 'Sim'
        WHEN av.sn_utiliza_sangue = 'N' THEN 'Não'
        ELSE 'Desconhecido'
    END AS UTILIZA_SANGUE,

    CASE 
        WHEN av.sn_utiliza_congelacao = 'S' THEN 'Sim'
        WHEN av.sn_utiliza_congelacao = 'N' THEN 'Não'
        ELSE 'Desconhecido'
    END AS UTILIZA_CONGELACAO,

    ta.DS_TIP_ANEST AS TIPO_ANESTESIA,

    CASE 
        WHEN a.sn_infeccao = 'S' THEN 'SIM'
        WHEN a.sn_infeccao = 'N' THEN 'NÃO'
        ELSE 'Desconhecido'
    END AS INFECCAO,

    csv.DATA_COLETA AS DATA_COLETA_SINAL_VITAL,
    sv.DS_SINAL_VITAL AS SINAL_VITAL,
    itcsv.VALOR AS VALOR_SINAL_VITAL

FROM ATENDIME a

INNER JOIN PACIENTE p ON a.CD_PACIENTE = p.CD_PACIENTE
LEFT JOIN MOT_ALT ma ON a.CD_MOT_ALT = ma.CD_MOT_ALT
LEFT JOIN CID c ON a.CD_CID = c.CD_CID
LEFT JOIN PED_LAB pl ON a.CD_ATENDIMENTO = pl.CD_ATENDIMENTO
LEFT JOIN RES_EXA re ON pl.CD_PED_LAB = re.CD_PED_LAB
LEFT JOIN AVISO_CIRURGIA av ON a.CD_ATENDIMENTO = av.CD_ATENDIMENTO AND av.TP_SITUACAO = 'R'  -- filtra para cirurgias realizadas
LEFT JOIN CIRURGIA_AVISO ca ON av.CD_AVISO_CIRURGIA = ca.CD_AVISO_CIRURGIA
LEFT JOIN CIRURGIA ci ON ca.CD_CIRURGIA = ci.CD_CIRURGIA
LEFT JOIN TIP_ANEST ta ON av.CD_TIP_ANEST = ta.CD_TIP_ANEST
LEFT JOIN COLETA_SINAL_VITAL csv ON a.CD_ATENDIMENTO = csv.CD_ATENDIMENTO
LEFT JOIN ITCOLETA_SINAL_VITAL itcsv ON csv.CD_COLETA_SINAL_VITAL = itcsv.CD_COLETA_SINAL_VITAL
LEFT JOIN SINAL_VITAL sv ON itcsv.CD_SINAL_VITAL = sv.CD_SINAL_VITAL

WHERE 
  a.TP_ATENDIMENTO = 'I'  -- filtra para internados
