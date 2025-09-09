<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Daftar Member</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="container">
        <h1>DAFTAR MEMBER </h1>
        <form id="memberForm">
            <input type="text" id="memberName" placeholder="Masukkan Nama Member" required>
            <button type="submit">Tambah Member</button>
        </form>
        <table id="memberTable">
            <thead>
                <tr>
                    <th>No.</th>
                    <th>Nama</th>
                    <th>Jumlah Kehadiran</th>
                    <th>Aksi</th>
                </tr>
            </thead>
            <tbody>
                </tbody>
        </table>
    </div>
    <script src="script.js"></script>
</body>
</html>

body {
    font-family: Arial, sans-serif;
    background-color: #f4f4f4;
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
    margin: 0;
}

.container {
    background-color: #fff;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    width: 90%;
    max-width: 600px;
    text-align: center;
}

h1 {
    color: #333;
}

#memberForm {
    display: flex;
    justify-content: center;
    margin-bottom: 20px;
}

#memberName {
    padding: 10px;
    border: 1px solid #ddd;
    border-radius: 4px;
    flex-grow: 1;
}

button {
    padding: 10px 15px;
    background-color: #5cb85c;
    color: #fff;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    margin-left: 10px;
}

button:hover {
    background-color: #4cae4c;
}

#memberTable {
    width: 100%;
    border-collapse: collapse;
    margin-top: 20px;
}

#memberTable th, #memberTable td {
    border: 1px solid #ddd;
    padding: 10px;
    text-align: left;
}

#memberTable th {
    background-color: #f2f2f2;
}

#memberTable tr:nth-child(even) {
    background-color: #f9f9f9;
}
document.addEventListener('DOMContentLoaded', () => {
    const memberForm = document.getElementById('memberForm');
    const memberNameInput = document.getElementById('memberName');
    const memberTableBody = document.querySelector('#memberTable tbody');
    
    // Memuat data dari Local Storage saat halaman dimuat
    let members = JSON.parse(localStorage.getItem('memberData')) || [];

    // Fungsi untuk merender (menampilkan) tabel
    function renderTable() {
        memberTableBody.innerHTML = ''; 
        
        members.sort((a, b) => a.name.localeCompare(b.name));

        members.forEach((member, index) => {
            const row = document.createElement('tr');
            row.innerHTML = `
                <td>${index + 1}</td>
                <td>${member.name}</td>
                <td>${member.count}</td>
                <td><button class="delete-btn" data-name="${member.name}">Hapus</button></td>
            `;
            memberTableBody.appendChild(row);
        });
    }

    // Fungsi untuk menyimpan data ke Local Storage
    function saveData() {
        localStorage.setItem('memberData', JSON.stringify(members));
    }

    // Event listener saat form disubmit
    memberForm.addEventListener('submit', (event) => {
        event.preventDefault(); 
        
        const memberName = memberNameInput.value.trim();
        
        if (memberName) {
            const existingMember = members.find(m => m.name.toLowerCase() === memberName.toLowerCase());

            if (existingMember) {
                existingMember.count++;
            } else {
                members.push({
                    name: memberName,
                    count: 1
                });
            }

            saveData();
            renderTable();
            memberNameInput.value = '';
        }
    });

    // Event listener untuk tombol hapus
    memberTableBody.addEventListener('click', (event) => {
        if (event.target.classList.contains('delete-btn')) {
            const nameToDelete = event.target.getAttribute('data-name');
            
            const memberIndex = members.findIndex(m => m.name === nameToDelete);

            if (memberIndex !== -1) {
                members.splice(memberIndex, 1);
                
                saveData();
                renderTable();
            }
        }
    });

    renderTable();
});
