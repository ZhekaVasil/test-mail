<?php
$server = stream_socket_server("tcp://localhost:25", $ErrNo, $ErrStr);
while($client = stream_socket_accept($server, -1))
{
    echo "New client detected\n";
    fwrite($client, "220 Hello client! I'm server Vista\r\n");
    while ($response_client = stream_get_line($client, 1000, "\r\n"))
    {
        echo $response_client . "\n";
        switch($response_client) {
            case "EHLO [192.168.56.1]":
                fwrite($client, "250 domain name should be qualified\r\n");
                break;
            case "RSET":
            case "MAIL FROM:<test@test>":
                fwrite($client, "250 OK\r\n");
                break;
            default:
                echo "CMD $response_client not found\n";
        }
    }

}
