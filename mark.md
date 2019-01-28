
django源码主要在core, db两个包，其中db包代码量

    db 662 lines
    
    db.models  7585 lines
    db.models.fields  6075 lines
    db.models.functions 773 lines
    db.models.sql 4320 lines
    
    db.backends 434 lines
    db.backends.base 3192 lines
    db.backends.mysql 1256 lines
    
    db.migrations 4261 lines
    db.migrations.operations 1562 lines
    
    total: 30120 lines
    
    涉及到元类、装饰器、描述器、inspect、yield、懒加载、type。
    getattr\hasattr\setattr
    import_module\
    copy\deepcopy
    主要由Manager、QuerySet、Query、SQLCompiler四个类完成CURD。
    查询是懒查询，由_fetch_all完成真正的查询工作。
    还涉及到ModelIterable。
    主要步骤：
        1. as_sql
        2. execute_sql
