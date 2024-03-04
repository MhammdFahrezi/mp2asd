# mp2asd
Nama:Muhammad Fahrezy,NIM:2309116988

class Node:
    def __init__(self, pajak):
        self.pajak = pajak
        self.next = None

class PajakLinkedList:
    def __init__(self):
        self.head = None

    def tambah_di_awal(self, pajak):
        new_node = Node(pajak)
        new_node.next = self.head
        self.head = new_node

    def tambah_di_akhir(self, pajak):
        new_node = Node(pajak)
        if not self.head:
            self.head = new_node
            return
        last = self.head
        while last.next:
            last = last.next
        last.next = new_node

    def tambah_di_antara(self, pajak, posisi):
        if posisi == 0:
            self.tambah_di_awal(pajak)
            return
        new_node = Node(pajak)
        temp = self.head
        for i in range(posisi - 1):
            if temp.next:
                temp = temp.next
            else:
                print("Posisi yang dimasukkan melebihi panjang list.")
                return
        new_node.next = temp.next
        temp.next = new_node

    def hapus_di_awal(self):
        if not self.head:
            print("List kosong. Tidak ada yang bisa dihapus.")
            return
        self.head = self.head.next

    def hapus_di_akhir(self):
        if not self.head:
            print("List kosong. Tidak ada yang bisa dihapus.")
            return
        if not self.head.next:
            self.head = None
            return
        second_last = self.head
        while second_last.next.next:
            second_last = second_last.next
        second_last.next = None

    def hapus_di_antara(self, posisi):
        if not self.head:
            print("List kosong. Tidak ada yang bisa dihapus.")
            return
        temp = self.head
        if posisi == 0:
            self.head = temp.next
            return
        for i in range(posisi - 1):
            if temp.next:
                temp = temp.next
            else:
                print("Posisi yang dimasukkan melebihi panjang list.")
                return
        if not temp or not temp.next:
            print("Posisi yang dimasukkan melebihi panjang list.")
            return
        temp.next = temp.next.next

    def tampilkan_list_pajak(self):
        temp = self.head
        if not temp:
            print("Tidak ada data pajak yang tersedia.")
        else:
            print("List Pajak:")
            while temp:
                pajak = temp.pajak
                print(f"No: {pajak.no}, Nama: {pajak.nama_pajak}, Terkena Pajak: {pajak.yang_terkena_pajak}, Penghasilan Pertahun: {pajak.penghasilan_pertahun}, Potongan Persen: {pajak.potongan_persen}")
                temp = temp.next


class Pajak:
    def __init__(self, no, nama_pajak, yang_terkena_pajak, penghasilan_pertahun, potongan_persen):
        self.no = no
        self.nama_pajak = nama_pajak
        self.yang_terkena_pajak = yang_terkena_pajak
        self.penghasilan_pertahun = penghasilan_pertahun
        self.potongan_persen = potongan_persen


class PajakManager:
    def __init__(self):
        self.pajak_list = PajakLinkedList()

    def tambah_pajak(self, pajak, posisi="akhir"):
        if posisi == "awal":
            self.pajak_list.tambah_di_awal(pajak)
        elif posisi == "akhir":
            self.pajak_list.tambah_di_akhir(pajak)
        else:
            posisi = int(posisi)
            self.pajak_list.tambah_di_antara(pajak, posisi)
        print(f"Pajak {pajak.nama_pajak} telah ditambahkan.")

    def tampilkan_list_pajak(self):
        self.pajak_list.tampilkan_list_pajak()

    def perbarui_pajak(self, no_pajak, field, value):
        temp = self.pajak_list.head
        while temp:
            if temp.pajak.no == no_pajak:
                setattr(temp.pajak, field, value)
                print("Data pajak telah diperbarui.")
                return
            temp = temp.next
        print("Nomor pajak tidak ditemukan.")

    def hapus_pajak(self, posisi="akhir"):
        if posisi == "awal":
            self.pajak_list.hapus_di_awal()
        elif posisi == "akhir":
            self.pajak_list.hapus_di_akhir()
        else:
            posisi = int(posisi)
            self.pajak_list.hapus_di_antara(posisi)
        print("Data pajak telah dihapus.")


def main():
    manager = PajakManager()
    pembayaran_pajak = [
        {"no": 1, "nama_pajak": "Pajak_PPh", "yang_terkena_pajak": "upah, gaji, tunjangan", "penghasilan_pertahun": 5000, "potongan_persen": "5%"},
        {"no": 2, "nama_pajak": "Pajak_PPn", "yang_terkena_pajak": "pengusaha, perusahaan", "penghasilan_pertahun": 2000, "potongan_persen": "5%"}
    ]
    
    for pajak_data in pembayaran_pajak:
        pajak_baru = Pajak(**pajak_data)
        manager.tambah_pajak(pajak_baru)
    
    while True:
        print("\nMenu Sistem Pendataan Perpajakan:")
        print("1. Tampilkan List Pajak")
        print("2. Tambah Pajak di Awal")
        print("3. Tambah Pajak di Akhir")
        print("4. Tambah Pajak di Tengah")
        print("5. Perbarui Pajak")
        print("6. Hapus Pajak di Awal")
        print("7. Hapus Pajak di Akhir")
        print("8. Hapus Pajak di Tengah")
        print("9. Keluar")

        pilihan = input("Pilih menu (1-9): ")

        if pilihan == "1":
            manager.tampilkan_list_pajak()
        elif pilihan in ["2", "3", "4"]:
            no = int(input("Masukkan nomor pajak: "))
            nama_pajak = input("Masukkan nama pajak: ")
            yang_terkena_pajak = input("Masukkan yang terkena pajak: ")
            penghasilan_pertahun = int(input("Masukkan penghasilan pertahun: "))
            potongan_persen = input("Masukkan potongan persen: ")
            posisi = input("Masukkan posisi (awal/akhir/antara): ")
            manager.tambah_pajak(Pajak(no, nama_pajak, yang_terkena_pajak, penghasilan_pertahun, potongan_persen), posisi)
        elif pilihan == "5":
            no_pajak = int(input("Masukkan nomor pajak yang akan diperbarui: "))
            field = input("Masukkan field yang akan diperbarui: ")
            value = input("Masukkan nilai baru: ")
            manager.perbarui_pajak(no_pajak, field, value)
        elif pilihan in ["6", "7", "8"]:
            posisi = input("Masukkan posisi (awal/akhir/antara): ")
            manager.hapus_pajak(posisi)
        elif pilihan == "9":
            print("Program berakhir.")
            break
        else:
            print("Pilihan tidak valid. Silakan coba lagi.")

if __name__ == "__main__":
    main()
