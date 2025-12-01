```json
{

    "payload": {

        "product": "Plano", // Plano, Plano Pet ou Seguro (Seguro está em desenvolvimento)

        //"pets": 2, // Somente para plano pet (quantidade de pets)

        //"tipo": "essencial", // Somente para plano pet (essencial, completo ou premium)

        "contratacao": {

            "Produto": 21, // Produto no Lis

            "Plano": 56, // No plano Pet, será automaticamente identificado com base nos campos pets e tipo

            "Corretor": 1,

            "DataInicioVigencia": "2025-11-20",

            "DataFimVigencia": "2026-02-20", // Para cadastro de clube de beneficios

            "FrequenciaEmissao": 1, // ANUAL, MENSAL...

            "TipoVencimento": 1,

            "DiaVencimento": 15,

            "AtividadePrincipal": 1, // CNAE

            "Segurado": {

                "Cpf": "06707357603",

                "Nome": "HECTOR DUTRA Black",

                "DataNascimento": "2003-04-09",

                "Capital": 0.01, // CAPITAL NAS ASSISTENCIAS

                "Genero": 1,

                "EstadoCivil": 1,

                "Endereco": {

                    "Endereco": "Rua Clemente Martins de Matos",

                    "Numero": "804",

                    "Complemento": null,

                    "Bairro": "Vila Itaim",

                    "Cidade": "São Paulo",

                    "UF": "SP",

                    "CEP": "08190430"

                },

                "Email": "hector.dutra@masterclassic.com.br",

                "PoliticamenteExposta": false

            },

            "FormaPagamento": 0, // Se forma de pagamento for 7 - Pix será trocada para 5 - transferência Eletrônica

            "Origem": 1

        }

    }

}
```