somesite
========

##Playing with Appex

**Appex https://github.com/sinclairzx81/appex is a really easy to use web framework, making use of Typescript in smart ways. It's for example compiling the typescript in memory, not using javascript files. As the compiles are in the memory changes are instant (in dev mode), just edit save and check out the results.**

Simply add modules with functions for each url endpoint. Appex will figure out url paths by the module and function names.

For example, a function named 'about' in the main module (usually saved in program.ts) will return data on the /about url:

    export function about(context:appex.web.IContext) {
        context.response.send('hello from about');
    }

Serv json by using the json function instead:

    // this will handle the url: /somedata
    export function somedata(context:appex.web.IContext) {
        context.response.json({id:0,foo:'bar'});
    }

To create a typical resource with some url endpoint, you just name a module with the resource name, and use functions within it.

Appex supports views too, just use express ones or the built in razor like view engine:


    export module products{
        ...

        // handling /products/:id url
        export function wildcard(context:appex.web.IContext, id:number){

            sql.open(conn_str, function( err, conn ) {
                conn.query( "SELECT * FROM dbo.name WHERE id=" + id, function( err, results ) {
                    var text = context.template.render('./views/products/item.txt', { model: results[0] });
                    context.response.headers['Content-Type'] = 'text/html';
                    context.response.send(text);
                });
            });

        }
    }

/views/products/item.txt:

    <p>Id: @(context.model.id)</p>
    <p>Value: @(context.model.value)</p>

    <a href="/products">List products</a>


There's **much** more to Appex. Be sure to check out the project itself https://github.com/sinclairzx81/appex

If you need the sql module it's at npm install msnodesql. Unfortunately they do not have updated binaries (node v1 & x64) yet, if you like those you need to compile yourself, or grab it from here https://github.com/joeriks/nodesql_sample
