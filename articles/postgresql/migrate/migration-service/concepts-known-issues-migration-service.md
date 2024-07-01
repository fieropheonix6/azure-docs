---
title: "Migration service - known issues and limitations"
description: Providing the limitations and known issues of the migration service in Azure Database for PostgreSQL.
author: apduvuri
ms.author: adityaduvuri
ms.reviewer: maghan
ms.date: 06/19/2024
ms.service: postgresql
ms.topic: conceptual
---

# Known issues and limitations for the migration service in Azure Database for PostgreSQL

[!INCLUDE [applies-to-postgresql-flexible-server](~/reusable-content/ce-skilling/azure/includes/postgresql/includes/applies-to-postgresql-flexible-server.md)]

This article describes the known issues and limitations associated with the migrations service in Azure Database for PostgreSQL.

## Common limitations

Here are common limitations that apply to migration scenarios:

- You can have only one active migration or validation to your Flexible server.

- The migration service only supports users and roles migration when the source is Azure Database for PostgreSQL single server.

- The migration service shows the number of tables copied from source to target. You must manually check the data and PostgreSQL objects on the target server post-migration.

- The migration service only migrates user databases, not system databases such as template_0 and template_1.

- The migration service doesn't support moving TIMESCALEDB, POSTGIS_TOPOLOGY, POSTGIS_TIGER_GEOCODER, PG_PARTMAN extensions from source to target.

- You can't move extensions not supported by the Azure Database for PostgreSQL – Flexible server. The supported extensions are listed in [Extensions - Azure Database for PostgreSQL](/azure/postgresql/flexible-server/concepts-extensions).

- User-defined collations can't be migrated into Azure Database for PostgreSQL – flexible server.

- You can't migrate to an older version. For instance, you can't migrate from PostgreSQL 15 to Azure Database for PostgreSQL version 14.

- The migration service only works with preferred or required SSLMODE values.

- The migration service doesn't support superuser privileges and objects.

- Azure Database for PostgreSQL - Flexible Server does not support the creation of custom tablespaces due to superuser privilege restrictions. During migration, data from custom tablespaces in the source PostgreSQL instance is migrated into the default tablespaces of the target Azure Database for PostgreSQL - Flexible Server.

- The following PostgreSQL objects can't be migrated into the PostgreSQL flexible server target:
    - Create casts
    - Creation of FTS parsers and FTS templates
    - Users with superuser roles
    - Create TYPE

- The migration service doesn't support migration at the object level, that is, at the table level or schema level.

- The migration service is unable to perform migration when the source database is Azure Database for PostgreSQL single server with no public access or is an on-premises/AWS using a private IP, and the target Azure Database for PostgreSQL Flexible Server is accessible only through a private endpoint.

- Migration to burstable SKUs isn't supported; databases must first be migrated to a non-burstable SKU and then scaled down if needed.

- The Migration Runtime Server is specifically designed to operate with the default DNS servers/private DNS zones i.e., **privatelink.postgres.database.azure.com**. Custom DNS names/DNS servers are not supported by the migration service when utilizing the migration runtime server feature. When configuring private endpoints for both the source and target databases, it is imperative to use the default private DNS zone provided by Azure for the private link service. The use of custom DNS configurations is not yet supported and may lead to connectivity issues during the migration process.

## Limitations migrating from Azure Database for PostgreSQL single server

- It's required that both the Azure Database for PostgreSQL single server and the Azure Database for PostgreSQL flexible server are in the same Azure region. Exceptions to this limitation are made for Azure Database for PostgreSQL flexible server situated in India, China, and UAE, where cross-region migrations are permitted.

- Microsoft Entra ID users present on your source server aren't migrated to the target server. To mitigate this limitation, visit [Manage Microsoft Entra roles](../../flexible-server/how-to-manage-azure-ad-users.md) to manually create all Microsoft Entra users on your target server before triggering a migration. If Microsoft Entra users aren't created on target server, migration fail.

- If the target flexible server uses SCRAM-SHA-256 password encryption method, connection to flexible server using the users/roles on single server fails since the passwords are encrypted using md5 algorithm. To mitigate this limitation, choose the option MD5 for password_encryption server parameter on your flexible server.

- Online migration makes use of [pgcopydb follow](https://pgcopydb.readthedocs.io/en/latest/ref/pgcopydb_follow.html) and some of the [logical decoding restrictions](https://pgcopydb.readthedocs.io/en/latest/ref/pgcopydb_follow.html#pgcopydb-follow) apply.

## Related content

- [Migration service](concepts-migration-service-postgresql.md)
- [Network setup](how-to-network-setup-migration-service.md)
- [Premigration validations](concepts-premigration-migration-service.md)
