# bug-prefect-greenlet

## Introduction

I met an issue that need install `greenlet` to be able to run Prefect workflow.

Track at https://github.com/PrefectHQ/prefect/issues/7978

## Observed behavior

To reproduce, run:

```shell
poetry install
poetry run python main.py
```

It will show something like:

```shell
➜ poetry run python main.py
Traceback (most recent call last):
  File "/bug-prefect-greenlet/main.py", line 24, in <module>
    greet(external_user)
  File "/Users/me/Library/Caches/pypoetry/virtualenvs/hm-prefect-H7-f_vW2-py3.11/lib/python3.11/site-packages/prefect/flows.py", line 448, in __call__
    return enter_flow_run_engine_from_flow_call(
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/me/Library/Caches/pypoetry/virtualenvs/hm-prefect-H7-f_vW2-py3.11/lib/python3.11/site-packages/prefect/engine.py", line 164, in enter_flow_run_engine_from_flow_call
    return anyio.run(begin_run)
           ^^^^^^^^^^^^^^^^^^^^
  File "/Users/me/Library/Caches/pypoetry/virtualenvs/hm-prefect-H7-f_vW2-py3.11/lib/python3.11/site-packages/anyio/_core/_eventloop.py", line 70, in run
    return asynclib.run(func, *args, **backend_options)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/me/Library/Caches/pypoetry/virtualenvs/hm-prefect-H7-f_vW2-py3.11/lib/python3.11/site-packages/anyio/_backends/_asyncio.py", line 292, in run
    return native_run(wrapper(), debug=debug)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/opt/homebrew/Cellar/python@3.11/3.11.0/Frameworks/Python.framework/Versions/3.11/lib/python3.11/asyncio/runners.py", line 190, in run
    return runner.run(main)
           ^^^^^^^^^^^^^^^^
  File "/opt/homebrew/Cellar/python@3.11/3.11.0/Frameworks/Python.framework/Versions/3.11/lib/python3.11/asyncio/runners.py", line 118, in run
    return self._loop.run_until_complete(task)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/opt/homebrew/Cellar/python@3.11/3.11.0/Frameworks/Python.framework/Versions/3.11/lib/python3.11/asyncio/base_events.py", line 650, in run_until_complete
    return future.result()
           ^^^^^^^^^^^^^^^
  File "/Users/me/Library/Caches/pypoetry/virtualenvs/hm-prefect-H7-f_vW2-py3.11/lib/python3.11/site-packages/anyio/_backends/_asyncio.py", line 287, in wrapper
    return await func(*args)
           ^^^^^^^^^^^^^^^^^
  File "/Users/me/Library/Caches/pypoetry/virtualenvs/hm-prefect-H7-f_vW2-py3.11/lib/python3.11/site-packages/prefect/client/utilities.py", line 45, in with_injected_client
    async with client_context as new_client:
  File "/Users/me/Library/Caches/pypoetry/virtualenvs/hm-prefect-H7-f_vW2-py3.11/lib/python3.11/site-packages/prefect/client/orion.py", line 1918, in __aenter__
    self._ephemeral_lifespan = await self._exit_stack.enter_async_context(
                               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/opt/homebrew/Cellar/python@3.11/3.11.0/Frameworks/Python.framework/Versions/3.11/lib/python3.11/contextlib.py", line 635, in enter_async_context
    result = await _enter(cm)
             ^^^^^^^^^^^^^^^^
  File "/opt/homebrew/Cellar/python@3.11/3.11.0/Frameworks/Python.framework/Versions/3.11/lib/python3.11/contextlib.py", line 204, in __aenter__
    return await anext(self.gen)
           ^^^^^^^^^^^^^^^^^^^^^
  File "/Users/me/Library/Caches/pypoetry/virtualenvs/hm-prefect-H7-f_vW2-py3.11/lib/python3.11/site-packages/prefect/client/base.py", line 90, in app_lifespan_context
    await context.__aenter__()
  File "/Users/me/Library/Caches/pypoetry/virtualenvs/hm-prefect-H7-f_vW2-py3.11/lib/python3.11/site-packages/asgi_lifespan/_manager.py", line 92, in __aenter__
    await self._exit_stack.aclose()
  File "/opt/homebrew/Cellar/python@3.11/3.11.0/Frameworks/Python.framework/Versions/3.11/lib/python3.11/contextlib.py", line 672, in aclose
    await self.__aexit__(None, None, None)
  File "/opt/homebrew/Cellar/python@3.11/3.11.0/Frameworks/Python.framework/Versions/3.11/lib/python3.11/contextlib.py", line 730, in __aexit__
    raise exc_details[1]
  File "/opt/homebrew/Cellar/python@3.11/3.11.0/Frameworks/Python.framework/Versions/3.11/lib/python3.11/contextlib.py", line 713, in __aexit__
    cb_suppress = await cb(*exc_details)
                  ^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/me/Library/Caches/pypoetry/virtualenvs/hm-prefect-H7-f_vW2-py3.11/lib/python3.11/site-packages/asgi_lifespan/_concurrency/asyncio.py", line 80, in __aexit__
    await self.task
  File "/Users/me/Library/Caches/pypoetry/virtualenvs/hm-prefect-H7-f_vW2-py3.11/lib/python3.11/site-packages/asgi_lifespan/_concurrency/asyncio.py", line 63, in run_and_silence_cancelled
    await self.coroutine()
  File "/Users/me/Library/Caches/pypoetry/virtualenvs/hm-prefect-H7-f_vW2-py3.11/lib/python3.11/site-packages/asgi_lifespan/_manager.py", line 64, in run_app
    await self.app(scope, self.receive, self.send)
  File "/Users/me/Library/Caches/pypoetry/virtualenvs/hm-prefect-H7-f_vW2-py3.11/lib/python3.11/site-packages/fastapi/applications.py", line 270, in __call__
    await super().__call__(scope, receive, send)
  File "/Users/me/Library/Caches/pypoetry/virtualenvs/hm-prefect-H7-f_vW2-py3.11/lib/python3.11/site-packages/starlette/applications.py", line 124, in __call__
    await self.middleware_stack(scope, receive, send)
  File "/Users/me/Library/Caches/pypoetry/virtualenvs/hm-prefect-H7-f_vW2-py3.11/lib/python3.11/site-packages/starlette/middleware/errors.py", line 149, in __call__
    await self.app(scope, receive, send)
  File "/Users/me/Library/Caches/pypoetry/virtualenvs/hm-prefect-H7-f_vW2-py3.11/lib/python3.11/site-packages/starlette/middleware/cors.py", line 76, in __call__
    await self.app(scope, receive, send)
  File "/Users/me/Library/Caches/pypoetry/virtualenvs/hm-prefect-H7-f_vW2-py3.11/lib/python3.11/site-packages/starlette/middleware/exceptions.py", line 55, in __call__
    await self.app(scope, receive, send)
  File "/Users/me/Library/Caches/pypoetry/virtualenvs/hm-prefect-H7-f_vW2-py3.11/lib/python3.11/site-packages/fastapi/middleware/asyncexitstack.py", line 21, in __call__
    raise e
  File "/Users/me/Library/Caches/pypoetry/virtualenvs/hm-prefect-H7-f_vW2-py3.11/lib/python3.11/site-packages/fastapi/middleware/asyncexitstack.py", line 18, in __call__
    await self.app(scope, receive, send)
  File "/Users/me/Library/Caches/pypoetry/virtualenvs/hm-prefect-H7-f_vW2-py3.11/lib/python3.11/site-packages/starlette/routing.py", line 695, in __call__
    await self.lifespan(scope, receive, send)
  File "/Users/me/Library/Caches/pypoetry/virtualenvs/hm-prefect-H7-f_vW2-py3.11/lib/python3.11/site-packages/starlette/routing.py", line 671, in lifespan
    async with self.lifespan_context(app):
  File "/Users/me/Library/Caches/pypoetry/virtualenvs/hm-prefect-H7-f_vW2-py3.11/lib/python3.11/site-packages/starlette/routing.py", line 566, in __aenter__
    await self._router.startup()
  File "/Users/me/Library/Caches/pypoetry/virtualenvs/hm-prefect-H7-f_vW2-py3.11/lib/python3.11/site-packages/starlette/routing.py", line 648, in startup
    await handler()
  File "/Users/me/Library/Caches/pypoetry/virtualenvs/hm-prefect-H7-f_vW2-py3.11/lib/python3.11/site-packages/prefect/orion/api/server.py", line 348, in run_migrations
    await db.create_db()
  File "/Users/me/Library/Caches/pypoetry/virtualenvs/hm-prefect-H7-f_vW2-py3.11/lib/python3.11/site-packages/prefect/orion/database/interface.py", line 55, in create_db
    await self.run_migrations_upgrade()
  File "/Users/me/Library/Caches/pypoetry/virtualenvs/hm-prefect-H7-f_vW2-py3.11/lib/python3.11/site-packages/prefect/orion/database/interface.py", line 63, in run_migrations_upgrade
    await run_sync_in_worker_thread(alembic_upgrade)
  File "/Users/me/Library/Caches/pypoetry/virtualenvs/hm-prefect-H7-f_vW2-py3.11/lib/python3.11/site-packages/prefect/utilities/asyncutils.py", line 91, in run_sync_in_worker_thread
    return await anyio.to_thread.run_sync(
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/me/Library/Caches/pypoetry/virtualenvs/hm-prefect-H7-f_vW2-py3.11/lib/python3.11/site-packages/anyio/to_thread.py", line 31, in run_sync
    return await get_asynclib().run_sync_in_worker_thread(
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/me/Library/Caches/pypoetry/virtualenvs/hm-prefect-H7-f_vW2-py3.11/lib/python3.11/site-packages/anyio/_backends/_asyncio.py", line 937, in run_sync_in_worker_thread
    return await future
           ^^^^^^^^^^^^
  File "/Users/me/Library/Caches/pypoetry/virtualenvs/hm-prefect-H7-f_vW2-py3.11/lib/python3.11/site-packages/anyio/_backends/_asyncio.py", line 867, in run
    result = context.run(func, *args)
             ^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/me/Library/Caches/pypoetry/virtualenvs/hm-prefect-H7-f_vW2-py3.11/lib/python3.11/site-packages/prefect/orion/database/alembic_commands.py", line 24, in wrapper
    return fn(*args, **kwargs)
           ^^^^^^^^^^^^^^^^^^^
  File "/Users/me/Library/Caches/pypoetry/virtualenvs/hm-prefect-H7-f_vW2-py3.11/lib/python3.11/site-packages/prefect/orion/database/alembic_commands.py", line 53, in alembic_upgrade
    alembic.command.upgrade(alembic_config(), revision, sql=dry_run)
  File "/Users/me/Library/Caches/pypoetry/virtualenvs/hm-prefect-H7-f_vW2-py3.11/lib/python3.11/site-packages/alembic/command.py", line 378, in upgrade
    script.run_env()
  File "/Users/me/Library/Caches/pypoetry/virtualenvs/hm-prefect-H7-f_vW2-py3.11/lib/python3.11/site-packages/alembic/script/base.py", line 569, in run_env
    util.load_python_file(self.dir, "env.py")
  File "/Users/me/Library/Caches/pypoetry/virtualenvs/hm-prefect-H7-f_vW2-py3.11/lib/python3.11/site-packages/alembic/util/pyfiles.py", line 94, in load_python_file
    module = load_module_py(module_id, path)
             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/me/Library/Caches/pypoetry/virtualenvs/hm-prefect-H7-f_vW2-py3.11/lib/python3.11/site-packages/alembic/util/pyfiles.py", line 110, in load_module_py
    spec.loader.exec_module(module)  # type: ignore
    ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "<frozen importlib._bootstrap_external>", line 940, in exec_module
  File "<frozen importlib._bootstrap>", line 241, in _call_with_frames_removed
  File "/Users/me/Library/Caches/pypoetry/virtualenvs/hm-prefect-H7-f_vW2-py3.11/lib/python3.11/site-packages/prefect/orion/database/migrations/env.py", line 147, in <module>
    apply_migrations()
  File "/Users/me/Library/Caches/pypoetry/virtualenvs/hm-prefect-H7-f_vW2-py3.11/lib/python3.11/site-packages/prefect/utilities/asyncutils.py", line 226, in coroutine_wrapper
    return run_async_from_worker_thread(async_fn, *args, **kwargs)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/me/Library/Caches/pypoetry/virtualenvs/hm-prefect-H7-f_vW2-py3.11/lib/python3.11/site-packages/prefect/utilities/asyncutils.py", line 177, in run_async_from_worker_thread
    return anyio.from_thread.run(call)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/me/Library/Caches/pypoetry/virtualenvs/hm-prefect-H7-f_vW2-py3.11/lib/python3.11/site-packages/anyio/from_thread.py", line 49, in run
    return asynclib.run_async_from_thread(func, *args)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/me/Library/Caches/pypoetry/virtualenvs/hm-prefect-H7-f_vW2-py3.11/lib/python3.11/site-packages/anyio/_backends/_asyncio.py", line 970, in run_async_from_thread
    return f.result()
           ^^^^^^^^^^
  File "/opt/homebrew/Cellar/python@3.11/3.11.0/Frameworks/Python.framework/Versions/3.11/lib/python3.11/concurrent/futures/_base.py", line 456, in result
    return self.__get_result()
           ^^^^^^^^^^^^^^^^^^^
  File "/opt/homebrew/Cellar/python@3.11/3.11.0/Frameworks/Python.framework/Versions/3.11/lib/python3.11/concurrent/futures/_base.py", line 401, in __get_result
    raise self._exception
  File "/Users/me/Library/Caches/pypoetry/virtualenvs/hm-prefect-H7-f_vW2-py3.11/lib/python3.11/site-packages/prefect/orion/database/migrations/env.py", line 137, in apply_migrations
    engine = await db_interface.engine()
             ^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/me/Library/Caches/pypoetry/virtualenvs/hm-prefect-H7-f_vW2-py3.11/lib/python3.11/site-packages/prefect/orion/database/interface.py", line 73, in engine
    engine = await self.database_config.engine()
             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/me/Library/Caches/pypoetry/virtualenvs/hm-prefect-H7-f_vW2-py3.11/lib/python3.11/site-packages/prefect/orion/database/configurations.py", line 221, in engine
    engine = create_async_engine(self.connection_url, echo=self.echo, **kwargs)
             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/me/Library/Caches/pypoetry/virtualenvs/hm-prefect-H7-f_vW2-py3.11/lib/python3.11/site-packages/sqlalchemy/ext/asyncio/engine.py", line 43, in create_async_engine
    sync_engine = _create_engine(*arg, **kw)
                  ^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "<string>", line 2, in create_engine
  File "/Users/me/Library/Caches/pypoetry/virtualenvs/hm-prefect-H7-f_vW2-py3.11/lib/python3.11/site-packages/sqlalchemy/util/deprecations.py", line 309, in warned
    return fn(*args, **kwargs)
           ^^^^^^^^^^^^^^^^^^^
  File "/Users/me/Library/Caches/pypoetry/virtualenvs/hm-prefect-H7-f_vW2-py3.11/lib/python3.11/site-packages/sqlalchemy/engine/create.py", line 660, in create_engine
    event.listen(pool, "connect", on_connect)
  File "/Users/me/Library/Caches/pypoetry/virtualenvs/hm-prefect-H7-f_vW2-py3.11/lib/python3.11/site-packages/sqlalchemy/event/api.py", line 115, in listen
    _event_key(target, identifier, fn).listen(*args, **kw)
  File "/Users/me/Library/Caches/pypoetry/virtualenvs/hm-prefect-H7-f_vW2-py3.11/lib/python3.11/site-packages/sqlalchemy/event/registry.py", line 232, in listen
    self.dispatch_target.dispatch._listen(self, *args, **kw)
  File "/Users/me/Library/Caches/pypoetry/virtualenvs/hm-prefect-H7-f_vW2-py3.11/lib/python3.11/site-packages/sqlalchemy/pool/events.py", line 69, in _listen
    event_key.base_listen(**kw)
  File "/Users/me/Library/Caches/pypoetry/virtualenvs/hm-prefect-H7-f_vW2-py3.11/lib/python3.11/site-packages/sqlalchemy/event/registry.py", line 270, in base_listen
    for_modify._set_asyncio()
  File "/Users/me/Library/Caches/pypoetry/virtualenvs/hm-prefect-H7-f_vW2-py3.11/lib/python3.11/site-packages/sqlalchemy/event/attr.py", line 265, in _set_asyncio
    self._exec_once_mutex = AsyncAdaptedLock()
                            ^^^^^^^^^^^^^^^^^^
  File "/Users/me/Library/Caches/pypoetry/virtualenvs/hm-prefect-H7-f_vW2-py3.11/lib/python3.11/site-packages/sqlalchemy/util/concurrency.py", line 67, in AsyncAdaptedLock
    _not_implemented()
  File "/Users/me/Library/Caches/pypoetry/virtualenvs/hm-prefect-H7-f_vW2-py3.11/lib/python3.11/site-packages/sqlalchemy/util/concurrency.py", line 47, in _not_implemented
    raise ValueError(
ValueError: the greenlet library is required to use this function. No module named 'greenlet'
```

## Expected behavior

I expect no need to install `greenlet` specifically.

## Workaround Way

Now if you install `greenlet` as a workaround way by

```shell
poetry add greenlet
poetry install
poetry run python main.py
```

The issue will be gone.

At this moment, if want to reproduce again, take macOS for example, need delete the Poetry cache folder such as at `rm -rf /Users/me/Library/Caches/pypoetry/virtualenvs/bug-prefect-greenlet-H7-f_vW2-py3.11`.
