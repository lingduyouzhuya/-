# -
//5.HOOK对象属性
lingdu.hookObj = function hookObj(obj, objName, propName, isDebug){
    // obj :需要hook的对象
    // objName: hook对象的名字
    // propName： 需要hook的对象属性名
    // isDubug: 是否需要debugger
    //1.获取属性描述符
    let oldDescriptor = Object.getOwnPropertyDescriptor(obj, propName);
    let newDescriptor = {};
    if(!oldDescriptor.configurable){ // 如果是不可配置的，直接返回
        return;
    }
    // 必须有的属性描述
    newDescriptor.configurable = true;

    if(oldDescriptor.hasOwnProperty("writable")){
        newDescriptor.writable = oldDescriptor.writable;
    }
    //是否存在 value 可能是值或者函数 我们只对函数进行hook
    if(oldDescriptor.hasOwnProperty("value")){
        let value = oldDescriptor.value;
        if(typeof value !== "function"){
            return;
        }
        let funcInfo = {
            "objName": objName,
            "funcName": propName
        }
        newDescriptor.value =lingdu.hook(value,funcInfo ,isDebug);
    }
    Object.defineProperty(obj, propName, newDescriptor);
}
