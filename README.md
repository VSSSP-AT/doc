# Documentação das Respostas da API de Mídia - Upload e Gerenciamento de Imagens

## 1. CRIAR MÍDIA (POST /media)

**Descrição:** Realiza o upload de uma nova imagem para o sistema.

**Requisição:**
- Método: POST
- Autenticação: Bearer Token (JWT)
- Content-Type: multipart/form-data
- Parâmetros:
  - `file`: Arquivo de imagem (obrigatório)
  - `assetName`: Nome personalizado para a mídia (opcional)
  - `assetType`: Tipo de mídia - valores permitidos: STORY, MAIN_BANNER, HOME_BANNER (opcional, padrão: STORY)
  - `linkUrl`: URL para associar à imagem (opcional)

**Resposta de Sucesso:**
- Status: 201 (Created)
- Corpo:
```json
{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "filename": "banner-promocional.jpg",
  "cloudflare_id": "cf_image_id_12345",
  "url": "https://imagedelivery.net/example/banner-promocional",
  "asset_type": "MAIN_BANNER",
  "link_url": "https://exemplo.com/promocao",
  "metadata": "{\"linkUrl\":\"https://exemplo.com/promocao\",\"customName\":\"Banner Promocional\"}",
  "user_id": "7f8c6d5e-4b3a-2c1d-0f9e-8d7c6b5a4b3a",
  "created_at": "2023-08-15T14:30:45.123Z",
  "updated_at": "2023-08-15T14:30:45.123Z"
}
```

**Respostas de Erro:**
- Status: 400 (Bad Request)
  ```json
  { "error": "Arquivo de mídia é obrigatório" }
  ```
  ou
  ```json
  { "error": "Tipo de ativo inválido. Deve ser um dos seguintes: STORY, MAIN_BANNER, HOME_BANNER" }
  ```

- Status: 401 (Unauthorized)
  ```json
  { "error": "Não autorizado" }
  ```

- Status: 500 (Internal Server Error)
  ```json
  { "error": "Erro ao fazer upload da mídia para o Cloudflare" }
  ```

## 2. OBTER MÍDIAS DO USUÁRIO (GET /media)

**Descrição:** Obtém a lista de mídias do usuário autenticado.

**Requisição:**
- Método: GET
- Autenticação: Bearer Token (JWT)
- Parâmetros de consulta:
  - `assetType`: Filtra por tipo de mídia (opcional) - valores permitidos: STORY, MAIN_BANNER, HOME_BANNER

**Resposta de Sucesso:**
- Status: 200 (OK)
- Corpo:
```json
[
  {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "filename": "banner-promocional.jpg",
    "cloudflare_id": "cf_image_id_12345",
    "url": "https://imagedelivery.net/example/banner-promocional",
    "asset_type": "MAIN_BANNER",
    "link_url": "https://exemplo.com/promocao",
    "metadata": "{\"linkUrl\":\"https://exemplo.com/promocao\",\"customName\":\"Banner Promocional\"}",
    "user_id": "7f8c6d5e-4b3a-2c1d-0f9e-8d7c6b5a4b3a",
    "created_at": "2023-08-15T14:30:45.123Z",
    "updated_at": "2023-08-15T14:30:45.123Z"
  },
  {
    "id": "661f9511-f30b-52e5-b827-557766551111",
    "filename": "story-exemplo.png",
    "cloudflare_id": "cf_image_id_67890",
    "url": "https://imagedelivery.net/example/story-exemplo",
    "asset_type": "STORY",
    "link_url": null,
    "metadata": "{\"linkUrl\":null,\"customName\":null}",
    "user_id": "7f8c6d5e-4b3a-2c1d-0f9e-8d7c6b5a4b3a",
    "created_at": "2023-08-16T10:15:30.456Z",
    "updated_at": "2023-08-16T10:15:30.456Z"
  }
]
```

**Respostas de Erro:**
- Status: 401 (Unauthorized)
  ```json
  { "error": "Não autorizado" }
  ```

## 3. OBTER MÍDIA POR ID (GET /media/{id})

**Descrição:** Obtém detalhes de uma mídia específica pelo ID.

**Requisição:**
- Método: GET
- Autenticação: Bearer Token (JWT)
- Parâmetros de caminho:
  - `id`: ID da mídia (UUID)

**Resposta de Sucesso:**
- Status: 200 (OK)
- Corpo:
```json
{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "filename": "banner-promocional.jpg",
  "cloudflare_id": "cf_image_id_12345",
  "url": "https://imagedelivery.net/example/banner-promocional",
  "asset_type": "MAIN_BANNER",
  "link_url": "https://exemplo.com/promocao",
  "metadata": "{\"linkUrl\":\"https://exemplo.com/promocao\",\"customName\":\"Banner Promocional\"}",
  "user_id": "7f8c6d5e-4b3a-2c1d-0f9e-8d7c6b5a4b3a",
  "created_at": "2023-08-15T14:30:45.123Z",
  "updated_at": "2023-08-15T14:30:45.123Z"
}
```

**Respostas de Erro:**
- Status: 401 (Unauthorized)
  ```json
  { "error": "Não autorizado" }
  ```

- Status: 404 (Not Found)
  ```json
  { "error": "Mídia não encontrada" }
  ```

## 4. ATUALIZAR MÍDIA (PUT /media/{id})

**Descrição:** Atualiza uma mídia existente pelo ID.

**Requisição:**
- Método: PUT
- Autenticação: Bearer Token (JWT)
- Content-Type: multipart/form-data
- Parâmetros de caminho:
  - `id`: ID da mídia (UUID)
- Parâmetros do corpo:
  - `file`: Novo arquivo de imagem (obrigatório)
  - `assetName`: Nome personalizado para a mídia (opcional)
  - `assetType`: Tipo de mídia - valores permitidos: STORY, MAIN_BANNER, HOME_BANNER (opcional)
  - `linkUrl`: URL para associar à imagem (opcional)

**Resposta de Sucesso:**
- Status: 200 (OK)
- Corpo:
```json
{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "filename": "banner-atualizado.jpg",
  "cloudflare_id": "cf_image_id_98765",
  "url": "https://imagedelivery.net/example/banner-atualizado",
  "asset_type": "MAIN_BANNER",
  "link_url": "https://exemplo.com/nova-promocao",
  "metadata": "{\"linkUrl\":\"https://exemplo.com/nova-promocao\",\"customName\":\"Banner Atualizado\"}",
  "user_id": "7f8c6d5e-4b3a-2c1d-0f9e-8d7c6b5a4b3a",
  "created_at": "2023-08-15T14:30:45.123Z",
  "updated_at": "2023-08-17T09:45:20.789Z"
}
```

**Respostas de Erro:**
- Status: 400 (Bad Request)
  ```json
  { "error": "Arquivo de mídia é obrigatório" }
  ```
  ou
  ```json
  { "error": "Tipo de ativo inválido. Deve ser um dos seguintes: STORY, MAIN_BANNER, HOME_BANNER" }
  ```

- Status: 401 (Unauthorized)
  ```json
  { "error": "Não autorizado" }
  ```

- Status: 403 (Forbidden)
  ```json
  { "error": "Você só pode atualizar suas próprias mídias" }
  ```

- Status: 404 (Not Found)
  ```json
  { "error": "Mídia não encontrada" }
  ```

## 5. EXCLUIR MÍDIA (DELETE /media/{id})

**Descrição:** Exclui uma mídia pelo ID.

**Requisição:**
- Método: DELETE
- Autenticação: Bearer Token (JWT)
- Parâmetros de caminho:
  - `id`: ID da mídia (UUID)

**Resposta de Sucesso:**
- Status: 204 (No Content)
- Sem corpo de resposta

**Respostas de Erro:**
- Status: 401 (Unauthorized)
  ```json
  { "error": "Não autorizado" }
  ```

- Status: 403 (Forbidden)
  ```json
  { "error": "Você só pode excluir suas próprias mídias" }
  ```

- Status: 404 (Not Found)
  ```json
  { "error": "Mídia não encontrada" }
  ```

---

**Observações:**
- As imagens são armazenadas no Cloudflare Images para distribuição otimizada
- O campo `metadata` armazena informações adicionais no formato JSON
- Todos os horários estão em UTC no formato ISO 8601 
