# 创建处理者链
basic = BasicSupport()
advanced = AdvancedSupport()
manager = ManagerSupport()

basic.set_next(advanced).set_next(manager)

