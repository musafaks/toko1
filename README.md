


1 . Jalankan Composer install atau composer Update
2 . Copy .env.example lalu setting sesuai database

3 . jalankan php artisan migrate 

3.1 . buka file LaravelServiceProvider di folder 	C:\xampp\htdocs\laravel-toko-online-master\vendor\kavist\rajaongkir\src\Providers  dan replace file  dengan contoh 1 

4 . jalankan php artisan db:seed (harus terkoneksi dengan internet soalnya mengambil data dari api raja ongkir)
5 . php artisan key:generate
6 . php artisan storage:link
7 . php artisan serve

8 . http://127.0.0.1:8000 halaman home
9 . http://127.0.0.1:8000/login halaman login 
	untuk login sebagai admin 
	user :  admin@sport.com
	password : rahasia





============== contoh 1
<?php

namespace Kavist\RajaOngkir\Providers;

use Illuminate\Support\ServiceProvider;
use Kavist\RajaOngkir\Exceptions\InvalidConfigurationException;
use Kavist\RajaOngkir\HttpClients\BasicClient;
use Kavist\RajaOngkir\RajaOngkir;
use Kavist\RajaOngkir\SearchDrivers\BasicDriver;

class LaravelServiceProvider extends ServiceProvider
{
    /**
     * @return void
     */
    public function boot()
    {
        $this->publishes([
            __DIR__.'/../../config/rajaongkir.php' => config_path('rajaongkir.php'),
        ]);
    }

    /**
     * @return void
     */
    public function register()
    {
        $this->mergeConfigFrom(__DIR__.'/../../config/rajaongkir.php', 'rajaongkir');

        $this->app->bind(BasicClient::class, function () {
            return new BasicClient;
        });

        $this->app->bind(BasicDriver::class, function () {
            return new BasicDriver;
        });

        $this->app->bind(RajaOngkir::class, function () {
            $config = config('rajaongkir');

            $this->guardAgainstInvalidConfiguration($config);

            return new RajaOngkir('f82c284a4c5a98ae9782c44b3ad52686', 'starter');
        });

        $this->app->alias(RajaOngkir::class, 'rajaongkir');
    }

    protected function guardAgainstInvalidConfiguration(array $config = null)
    {
        // return $config['api_key'];
        // if (empty($config['api_key'] || 'some32charstring' === $config['api_key'])) {
        //     throw InvalidConfigurationException::apiKeyNotSpecified();
        // }

        // if (! in_array($config['package'], ['starter', 'basic', 'pro'])) {
        //     throw InvalidConfigurationException::invalidApiPackage();
        // }
    }
}

==============
