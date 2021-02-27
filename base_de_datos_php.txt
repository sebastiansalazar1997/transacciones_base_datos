<?php
$cuenta_primaria= $_POST['cuenta_primaria'];
$cuenta_secundaria= $_POST['cuenta_secundaria'];
$valor= $_POST['valor'];
?>

<?php

try {
  $mbd = new PDO('mysql:host=localhost;dbname=bancosebas','root','root',
      array(PDO::ATTR_PERSISTENT => true));
  echo "Transaccion exitosa bb ♥ \n";
} catch (Exception $e) {
  die("NO SE HA PODIDO COMPLETA LA TRANSACCION " . $e->getMessage());
}

try {  
  $mbd->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);

  $mbd->beginTransaction();
  $mbd->exec("UPDATE cuenta SET saldo=saldo-$valor WHERE n_cuenta=$cuenta_primaria");
  $mbd->exec("UPDATE cuenta SET saldo=saldo+$valor WHERE n_cuenta=$cuenta_secundaria"); 
      
  $mbd->commit();
  
} catch (Exception $e) {
  $mbd->rollBack();
  echo "ERROR EN TRANSACCION " . $e->getMessage();
}
?>