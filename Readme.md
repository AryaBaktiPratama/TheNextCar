# The Next Car Apps
Merupakan aplikasi dashboard electric car yang memiliki konsep MVC (Model View Controller).

MVC sendiri berfungsi untuk

- Model berfungsi untuk mengatur data, fungsi, dan aturan dari aplikasi
- View berfungsi untuk mengatur tampilan/output yang tampil di layar
- Controller berfungsi untuk mengatur / meneruma input dan mmenjalankan beberapa perintah untuk dijalankan.
## Scope & Functionalities
- User dapat menyalakan tombol power ON/OFF pada tampilan aplikasi
- User dapat membuka / menutup pintu(door)
- User dapat mengkunci / membuka security lock pada car
- User dapat menjalankan mesin ketika semuanya sudah benar/ready
- Memiliki keamanan berkendara karena akan muncul pemberitahuan jika ada pintu yang belum tertutup / terkunci
 ## How Does It Works ?
Mobil / Car hanya dapat menyala jika kita sudah menutup pintu , mengunci pintu, dan menyalakan tombol ON.
```csharp
public void toggleStartEngineButton()
        {
            if (!doorIsClose())
            {
                this.callbackCarEngineStatusChanged.carEngineStatusChanged("STOPPED", "door is open");
                return;
            }
            if (!doorIsLocked())
            {
                this.callbackCarEngineStatusChanged.carEngineStatusChanged("STOPPED", "door is unlocked");
                return;
            }
            if (!powerIsReady())
            {
                this.callbackCarEngineStatusChanged.carEngineStatusChanged("STOPPED", "no power available");
                return;
            }
            this.callbackCarEngineStatusChanged.carEngineStatusChanged("STARTED", "Engine Started");
        }
```
## Apa kegunaan `DoorController.cs` ?
- `DoorController.cs` berfungsi untuk mengetahui dan memberikan informasi kondisi dari pintu mobil (open/ close/ lock/ unlock)
```csharp
namespace TheNextCar.Controller
{
    class DoorController
    {
        private Door door;
        private OnDoorChanged onDoorChanged;

        public DoorController(OnDoorChanged onDoorChanged)
        {
            this.door = new Door();
            this.onDoorChanged = onDoorChanged;
        }

        public void close()
        {
            this.door.close();
            this.onDoorChanged.doorStatusChanged("CLOSED", "Door is Closed");
        }

        public void open()
        {
            this.door.open();
            this.onDoorChanged.doorStatusChanged("OPENED", "Door is Opened");
        }

        public void activateLock()
        {
            if (this.door.isClosed())
            {
                this.door.activateLock();
                onDoorChanged.doorSecurityChanged("LOCKED", "Door Locked");
            }
            else
            {
                unlock();
            }
        }

        public void unlock()
        {
            this.door.unlock();
            onDoorChanged.doorSecurityChanged("UNLOCKED", "Door Unlocked");
        }
        public bool isClose()
        {
            return this.door.isClosed();
        }
        public bool isLocked()
        {
            return this.door.isLocked();
        }
    }
    interface OnDoorChanged
    {
        void doorSecurityChanged(string vale, string message);
        void doorStatusChanged(string vale, string message);

    }
}
```
## Apa kegunaan model `Door.cs` ?
- `Door.cs` berfungsi untuk mengetahui fungsi dari door Closed atau Locked
```csharp
namespace TheNextCar.Model
{
    class Door
    {
        private bool closed;
        private bool locked;

        public void close()
        {
            this.closed = true;
        }

        public void open()
        {
            this.closed = false;
        }

        public void activateLock()
        {
            this.locked = true;
        }
        public void unlock()
        {
            this.locked = false;
        }

        public bool isLocked()
        {
            return this.locked;
        }

        public bool isClosed()
        {
            return this.closed;
        }
    }

}
```
## Apa kegunaan interface `OnDoorChanged` ?
- interface `OnDoorChanged` berfungsi untuk mengganti fungsi dari `Door` dan `DoorController`.