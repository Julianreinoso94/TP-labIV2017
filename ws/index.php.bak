<?php
include("../clases/usuario.php");
include("../clases/ofertas.php");
include("../clases/productos.php");
include("../clases/sucursal.php");
include("../clases/pedidos.php");
require 'vendor/autoload.php';
 
 
$app = new Slim\App();

 


$app->get('/usuarios[/]', function ($request, $response, $args) {
    //$response->write("Lista de usuarios");
    $dato=usuario::TraerTodosLosusuarios();
    $response->write(json_encode($dato));

    return $response;
});
$app->get('/ofertas[/]', function ($request, $response, $args) {
 
    $dato=ofertas::TraerTodosLasofertas();
    $response->write(json_encode($dato));

    return $response;
});

$app->get('/sucursales[/]', function ($request, $response, $args) {
 
    $dato=sucursal::TraerTodosLasSucursales();
    $response->write(json_encode($dato));

    return $response;
});


$app->get('/Estadisticas[/{objeto}]', function ($request, $response, $args) {
 
 if ($args['objeto']=="1") {
    $resp=pedidos::cantPedPorSucursal();
    return json_encode($resp);
  }
    elseif ($args['objeto']=="2") {
     $resp=pedidos::cantPedPorSucursalYemp();
      return json_encode($resp);
    }
});
$app->get('/productos[/]', function ($request, $response, $args) {
 
    $dato=productos::TraerTodosLosproductos();
    $response->write(json_encode($dato));

    return $response;
});

$app->get('/sucursalesUp[/{objeto}]', function ($request, $response, $args) {
  

    $sucursal=json_decode($args['objeto']);
    $sucursal->fotos=explode(';',$sucursal->fotos);
    $arrayFoto = array();
    if(count($sucursal->fotos) > 0){
        for ($i = 0; $i < count($sucursal->fotos); $i++ ){
            $rutaVieja="imagenes/".$sucursal->fotos[$i];
            $rutaNueva=$sucursal->nombre. "_". $i .".".PATHINFO($rutaVieja, PATHINFO_EXTENSION);
            //copy($rutaVieja, "fotos/".$rutaNueva);
            //unlink($rutaVieja);
            //$arrayFoto[]="http://localhost:8026/TpLab4_2016/imagenes/Sucursales/".$rutaNueva;
             return json_encode($rutaNueva);
        } 

        //$sucursal->fotos=json_encode($arrayFoto); 

    }
   
});

$app->get('/OfertasDeSucursal[/{objeto}]', function ($request, $response, $args) {
  
    $sucursal=json_decode($args['objeto']);
    $res=ofertas::TraerofertasDeSucursal($sucursal);
    return json_encode($res);  
});

 

$app->get('/pedidos[/{objeto}]', function ($request, $response, $args) {

     if ($args!=null) {
        $pers=$args['objeto'];
        $idUser=(int)$pers;
        $resp=pedidos::TraerMisPedidos($idUser);
        return json_encode($resp);
        //return "vino el id";
     }else{
         $dato=pedidos::TraerTodosLosPedidos();
         $response->write(json_encode($dato));

         return $response;
        //return "no vino el id";
    }
 
  
   

  
});
$app->get('/pedidosDT[/{objeto}]', function ($request, $response, $args) {
  $pers=$args['objeto'];

 
  $idUser=(int)$pers;
  $resp=pedidos::TraerProdDeMisPedidos($idUser);
  return json_encode($resp);
   

});

$app->post('/login/{objeto}', function ($request, $response, $args) {
    
   

    $pers=$args['objeto'];
    $str=json_decode($pers);
    $dato=usuario::TraerUnusuario($pers);   
   echo json_encode($dato);
});

$app->post('/GuardarLogin/{objeto}', function ($request, $response, $args) {
    
    $pers=json_decode($args['objeto']);
    date_default_timezone_set('America/Argentina/Buenos_Aires');  
 
    $mail=$pers->mail;
    $nombre=$pers->nombre;
    $tipo=$pers->tipo;
    $fecha=date("Y-m-d H:i:s",time());
    $resp=usuario::GuardarLogin($mail,$nombre,$tipo,$fecha);
    echo json_encode($resp);

});


$app->get('/empleados[/]', function ($request, $response, $args) {
 
    $dato=usuario::TraerTodosLosempleados();
    $response->write(json_encode($dato));
    return $response;
});

$app->get('/encargado[/]', function ($request, $response, $args) {
 
    $dato=usuario::TraerTodosLosencargados();
    $response->write(json_encode($dato));
    return $response;
});

$app->get('/clientes[/]', function ($request, $response, $args) {
 
    $dato=usuario::TraerTodosLosclientes();
    $response->write(json_encode($dato));
    return $response;
});
$app->post('/AltaClientes/{objeto}', function ($request, $response, $args) {
 
    $pers=json_decode($args['objeto']);
    $UnCliente = new usuario();
    $UnCliente->id_user='4';
    $UnCliente->nombre = $pers->nombre;
    $UnCliente->apellido = $pers->apellido;
    $UnCliente->direccion=$pers->dir;
    $UnCliente->mail=$pers->mail;
    $UnCliente->telefono=$pers->tel;
    $UnCliente->tipo=$pers->tipo;
    $UnCliente->estado=$pers->estado;
    $UnCliente->sucursal="na";
    $UnCliente->password=$pers->pass;
    $dato=usuario::Insertarusuario($UnCliente);  
    $response->write(json_encode($dato));
    //return json_encode($UnCliente);
    return $response;

    
});

    $app->post('/AltaP/{objeto}', function ($request, $response, $args) {
 
    $prod=json_decode($args['objeto']);
    $UnProd = new productos();
    $UnProd->descripcion = $prod->descripcion;
    $UnProd->precio = $prod->precio;
    $dato=productos::Insertarproducto($UnProd);  
    $response->write(json_encode($dato));
    //return json_encode($UnProd);
    return $response;

    
});

 $app->post('/AltaOfertas/{objeto}', function ($request, $response, $args) {
 
    $of=json_decode($args['objeto']);
    $UnaOferta = new ofertas();
    $UnaOferta->descripcion = $of->descripcion;
    $UnaOferta->precio = $of->precio;
    $dato=sucursal::TraerUnasucursal($of->sucursal);
    $UnaOferta->id_local=$dato->id_sucursal;
    $resp=ofertas::Insertaroferta($UnaOferta);
 
    return json_encode($UnaOferta);
    
});

 $app->post('/ModifOfertas/{objeto}', function ($request, $response, $args) {
 
    $of=json_decode($args['objeto']);
      
    $UnaOferta = new ofertas();
    $UnaOferta->id_oferta=$of->id_oferta;
    $UnaOferta->descripcion = $of->descripcion;
    $UnaOferta->precio = $of->precio;
    $dato=sucursal::TraerUnasucursal($of->sucursal);
    $UnaOferta->id_local=$dato->id_sucursal;
    $resp=ofertas::Modificaroferta($UnaOferta);
 
    return json_encode($resp);
     
    //return json_encode($UnaOferta);
    
});


 $app->post('/ElimOfertas/{objeto}', function ($request, $response, $args) {
 
    $of=json_decode($args['objeto']); 
    $resp=ofertas::Borraroferta($of->id_oferta);

    return json_encode($resp);
     
    
});
 $app->post('/AltaPed/{objeto}', function ($request, $response, $args) {
 

    $prod=json_decode($args['objeto']);
   
    date_default_timezone_set('America/Argentina/Buenos_Aires');    
    $UnPedido = new pedidos();
    $UnPedido->lista_productos = $prod->lista_productos;
    $UnPedido->total_pedido = $prod->total_pedido;
    $UnPedido->id_user=$prod->id_user;
    $UnPedido->fecha=date("Y-m-d H:i:s",time());
    $UnPedido->sucursal=$prod->sucursal;
    $UnPedido->FechaEntrega=$prod->FechaEntrega;
    $dato=pedidos::InsertarPedidos($UnPedido);  
    $response->write(json_encode($dato));

    $idp=pedidos::TraerUltimoId();
             
      foreach ($prod->lista_productos as $p ) 
      {
        $idpr = $p->idp;
        $cantidad=$p->cant;
        $res=pedidos::InsertarDetPed($idpr,$idp->id_pedidos,$cantidad);
      }    
      //return $response;      
    return json_encode($UnPedido->fecha);
});

$app->post('/ModifUs/{objeto}', function ($request, $response, $args) {
 
    $pers=json_decode($args['objeto']);
    $UnCliente = new usuario();
    $UnCliente->id_user = $pers->id_user;
    $UnCliente->nombre = $pers->nombre;
    $UnCliente->apellido = $pers->apellido;
    $UnCliente->direccion=$pers->dir;
    $UnCliente->mail=$pers->mail;
    $UnCliente->telefono=$pers->tel;
    $UnCliente->tipo=$pers->tipo;
    $UnCliente->estado=$pers->estado;
    $UnCliente->sucursal=$pers->sucursal;
    $UnCliente->password=$pers->pass;
    $dato=usuario::Modificarusuario($UnCliente);  
    $response->write(json_encode($dato));
    //return json_encode($UnCliente);
    return $response;

    
});


$app->post('/ModifP/{objeto}', function ($request, $response, $args) {
 
    $prod=json_decode($args['objeto']);
    $UnProd = new productos();
    $UnProd->id_producto = $prod->id_producto;
    $UnProd->descripcion = $prod->descripcion;
    $UnProd->precio = $prod->precio;
    $dato=productos::Modificarproducto($UnProd);  
    $response->write(json_encode($dato));
    return $response;
    //return json_encode($UnProd);
});


$app->post('/ElimP/{objeto}', function ($request, $response, $args) {
 
    $prod=json_decode($args['objeto']);
    $UnProd = new productos();
    $UnProd->id_producto = $prod->id_producto;
    $dato=productos::Borrarproducto($UnProd->id_producto);  
    $response->write(json_encode($dato));
    return $response;
    return json_encode($UnProd);
});



$app->post('/ElimUs/{objeto}', function ($request, $response, $args) {
 
    $pers=json_decode($args['objeto']);
    $UnCliente = new usuario();
    $UnCliente->id_user = $pers->id_user;
    $UnCliente->nombre = $pers->nombre;
    $UnCliente->apellido = $pers->apellido;
    $UnCliente->direccion=$pers->dir;
    $UnCliente->mail=$pers->mail;
    $UnCliente->telefono=$pers->tel;
    $UnCliente->tipo=$pers->tipo;
    $UnCliente->estado=$pers->estado;
    $UnCliente->sucursal="na";
    $UnCliente->password=$pers->pass;
    $dato=usuario::Borrarusuario($UnCliente->mail,$UnCliente->id_user,$UnCliente->tipo);  
    $response->write(json_encode($dato));
    //return json_encode($UnCliente);
    return $response;

    
});

$app->post('/AltaUs/{objeto}', function ($request, $response, $args) {
 
    $pers=json_decode($args['objeto']);
    $UnCliente = new usuario();
    $UnCliente->id_user='4';
    $UnCliente->nombre = $pers->nombre;
    $UnCliente->apellido = $pers->apellido;
    $UnCliente->direccion=$pers->dir;
    $UnCliente->mail=$pers->mail;
    $UnCliente->telefono=$pers->tel;
    $UnCliente->tipo=$pers->tipo;
    $UnCliente->estado=$pers->estado;
    $UnCliente->sucursal=$pers->sucursal;
    $UnCliente->password=$pers->pass;
    //return json_encode($pers);
    $dato=usuario::Insertarusuario($UnCliente);  
    $response->write(json_encode($dato));
    return $response;

    
});


/*

$app->get('/', function ($request, $response, $args) {
    $response->write("Welcome to Slim!");
    return $response;
});

$app->post('/alta/{objeto}', function ($request, $response, $args) {
    
    $pers=json_decode($args['objeto']);
    $dato=Persona::InsertarPersona($pers);
    //$response->write(json_encode($dato));
    $response->write($args);
    return $response;
});


$app->get('/usuario[/{id}[/{name}]]', function ($request, $response, $args) {
    $response->write("Datos usuario ");
    var_dump($args);
    return $response;
});
 
$app->post('/usuario/{id}', function ($request, $response, $args) {
    $response->write("Welcome to Slim!");
    var_dump($args);
    return $response;
});

 
$app->put('/usuario/{id}', function ($request, $response, $args) {
    $response->write("Welcome to Slim!");
    var_dump($args);
    return $response;
});

 
$app->delete('/usuario/{id}', function ($request, $response, $args) {
    $response->write("borrar !", $args->id);
    var_dump($args);
    return $response;
});
 

 */
$app->run();
