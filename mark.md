
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
    getattr\hasattr\setattr\import_module\copy\deepcopy
    主要由Manager、QuerySet、Query、SQLCompiler四个类完成CURD。
    查询是懒查询，由_fetch_all完成真正的查询工作。
    还涉及到ModelIterable。
    主要步骤：
        1. as_sql
        2. execute_sql
        
        
get_wsgi_application()
   1. django.setup
      - 1.1 settings 
         - LazySettings() -- LazySettings(LazyObject)
         - 懒加载：先加载global_settings，后加载自定义settings_module
      - 1.2 configure_logging
      - 1.3 apps.populate(settings.INSTALLED_APPS)（thread-safe）
         - self.app_configs -- AppConfig可作钩子，加载每个app下的models模块
           初始化models.py中的model过程：
               - 调用元类ModelBase__new__方法，获取该model的app_config，添加_meta属性（Options实例）
                   - 将每个Field实例绑定到model，并把Field实例添加到Options实例的属性中。
                   
                       self.set_attributes_from_name(name)
                       cls._meta.add_field(self)
                       setattr(cls, self.attname, DeferredAttribute(self.attname))
                       基于contribute_to_class，有contribute_to_class的Field类：
                            Field、AutoField、DateField、
                            RelatedField、ForeignObject、ManyToManyField
                       ForeignKey实例的remote_field是ManyToOneRel
                       
                   - 自动添加主键AutoField
                   - 自动添加objects，即Manager实例
                     objects是QuerySet的代理。
                   - register_model in apps   
                     
   2. return WSGIHandler()
     load_middleware漏斗模型
     
request process 
             
