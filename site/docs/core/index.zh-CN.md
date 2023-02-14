---

function error(){
    return async (ctx,next) => {
        try {
            await next() 
            return
        } catch (error) {
            const status = error.status || 500
            
            ctx.body = {
                code:status,
                msg: error.message
            }
            ctx.app.emit('error',error, ctx )

            if(status == 401){
                ctx.body = {
                    code:status,
                    msg: '鉴权失败，请检查token是否传递或者有效'
                }
                return
            }
        }
    }
}


module.exports = error
