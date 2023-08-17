# EF Core 异常

## 异常 update-database

```
An error occurred using the connection to database '' on server '127.0.0.1'.

and

An exception has been raised that is likely due to a transient failure. Consider enabling transient error resiliency by adding 'EnableRetryOnFailure()' to the 'UseMySql' call.
```
```
PM> update-database city
Build started...
Build succeeded.
Microsoft.EntityFrameworkCore.Infrastructure[10403]
      Entity Framework Core 6.0.0 initialized 'MyContext' using provider 'Pomelo.EntityFrameworkCore.MySql:6.0.0' with options: ServerVersion 6.0.0-mysql 
fail: Microsoft.EntityFrameworkCore.Database.Connection[20004]
      An error occurred using the connection to database '' on server '127.0.0.1'.
An error occurred using the connection to database '' on server '127.0.0.1'.
System.InvalidOperationException: An exception has been raised that is likely due to a transient failure. Consider enabling transient error resiliency by adding 'EnableRetryOnFailure()' to the 'UseMySql' call.
 ---> MySqlConnector.MySqlException (0x80004005): Authentication method 'caching_sha2_password' failed. Either use a secure connection, specify the server's RSA public key with ServerRSAPublicKeyFile, or set AllowPublicKeyRetrieval=True.
   at MySqlConnector.Core.ServerSession.GetRsaPublicKeyAsync(String switchRequestName, ConnectionSettings cs, IOBehavior ioBehavior, CancellationToken cancellationToken) in /_/src/MySqlConnector/Core/ServerSession.cs:line 810
   at MySqlConnector.Core.ServerSession.SwitchAuthenticationAsync(ConnectionSettings cs, String password, PayloadData payload, IOBehavior ioBehavior, CancellationToken cancellationToken) in /_/src/MySqlConnector/Core/ServerSession.cs:line 687
   at MySqlConnector.Core.ServerSession.ConnectAsync(ConnectionSettings cs, MySqlConnection connection, Int32 startTickCount, ILoadBalancer loadBalancer, IOBehavior ioBehavior, CancellationToken cancellationToken) in /_/src/MySqlConnector/Core/ServerSession.cs:line 519
   at MySqlConnector.MySqlConnection.CreateSessionAsync(ConnectionPool pool, Int32 startTickCount, Nullable`1 ioBehavior, CancellationToken cancellationToken) in /_/src/MySqlConnector/MySqlConnection.cs:line 922
   at MySqlConnector.MySqlConnection.OpenAsync(Nullable`1 ioBehavior, CancellationToken cancellationToken) in /_/src/MySqlConnector/MySqlConnection.cs:line 405
   at MySqlConnector.MySqlConnection.Open() in /_/src/MySqlConnector/MySqlConnection.cs:line 369
   at Microsoft.EntityFrameworkCore.Storage.RelationalConnection.OpenDbConnection(Boolean errorsExpected)
   at Microsoft.EntityFrameworkCore.Storage.RelationalConnection.OpenInternal(Boolean errorsExpected)
   at Microsoft.EntityFrameworkCore.Storage.RelationalConnection.Open(Boolean errorsExpected)
   at Pomelo.EntityFrameworkCore.MySql.Storage.Internal.MySqlRelationalConnection.Open(Boolean errorsExpected)
   at Pomelo.EntityFrameworkCore.MySql.Storage.Internal.MySqlDatabaseCreator.<>c__DisplayClass18_0.<Exists>b__0(DateTime giveUp)
   at Microsoft.EntityFrameworkCore.ExecutionStrategyExtensions.<>c__DisplayClass12_0`2.<Execute>b__0(DbContext c, TState s)
   at Pomelo.EntityFrameworkCore.MySql.Storage.Internal.MySqlExecutionStrategy.Execute[TState,TResult](TState state, Func`3 operation, Func`3 verifySucceeded)
   --- End of inner exception stack trace ---
   at Pomelo.EntityFrameworkCore.MySql.Storage.Internal.MySqlExecutionStrategy.Execute[TState,TResult](TState state, Func`3 operation, Func`3 verifySucceeded)
   at Microsoft.EntityFrameworkCore.ExecutionStrategyExtensions.Execute[TState,TResult](IExecutionStrategy strategy, TState state, Func`2 operation, Func`2 verifySucceeded)
   at Microsoft.EntityFrameworkCore.ExecutionStrategyExtensions.Execute[TState,TResult](IExecutionStrategy strategy, TState state, Func`2 operation)
   at Pomelo.EntityFrameworkCore.MySql.Storage.Internal.MySqlDatabaseCreator.Exists(Boolean retryOnNotExists)
   at Pomelo.EntityFrameworkCore.MySql.Storage.Internal.MySqlDatabaseCreator.Exists()
   at Microsoft.EntityFrameworkCore.Migrations.HistoryRepository.Exists()
   at Microsoft.EntityFrameworkCore.Migrations.Internal.Migrator.Migrate(String targetMigration)
   at Microsoft.EntityFrameworkCore.Design.Internal.MigrationsOperations.UpdateDatabase(String targetMigration, String connectionString, String contextType)
   at Microsoft.EntityFrameworkCore.Design.OperationExecutor.UpdateDatabaseImpl(String targetMigration, String connectionString, String contextType)
   at Microsoft.EntityFrameworkCore.Design.OperationExecutor.UpdateDatabase.<>c__DisplayClass0_0.<.ctor>b__0()
   at Microsoft.EntityFrameworkCore.Design.OperationExecutor.OperationBase.Execute(Action action)
An exception has been raised that is likely due to a transient failure. Consider enabling transient error resiliency by adding 'EnableRetryOnFailure()' to the 'UseMySql' call.
PM>
```

解决方案


