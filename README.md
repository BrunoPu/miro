import { Component } from '@angular/core';
import { FormBuilder, FormGroup } from '@angular/forms'; // Importa o FormBuilder e FormGroup para criar e gerenciar o formulário
import { HttpClient } from '@angular/common/http'; // Para fazer requisições HTTP

@Component({
  selector: 'app-file-upload-form',
  templateUrl: './file-upload-form.component.html',
  styleUrls: ['./file-upload-form.component.css']
})
export class FileUploadFormComponent {
  form: FormGroup; // Grupo de formulários para armazenar todas as respostas
  base64File: string | null = null; // Para armazenar o arquivo convertido em base64

  constructor(private fb: FormBuilder, private http: HttpClient) {
    // Cria o formulário com FormBuilder
    this.form = this.fb.group({
      name: [''], // Exemplo de campo de texto
      email: [''], // Exemplo de campo de e-mail
      file: ['']   // Campo que será usado para armazenar o arquivo em base64
    });
  }

  // Método chamado quando um arquivo é selecionado
  onFileSelected(event: any) {
    const file: File = event.target.files[0]; // Obtém o arquivo selecionado

    // Verifica se o arquivo é um .msg
    if (file && file.type === "") {
      this.convertToBase64(file); // Converte o arquivo para base64
    } else {
      console.error('Por favor, selecione um arquivo .msg válido.');
    }
  }

  // Converte o arquivo para base64 e armazena no formulário
  convertToBase64(file: File) {
    const reader = new FileReader(); // Usa FileReader para ler o arquivo

    reader.onload = () => {
      this.base64File = (reader.result as string).split(',')[1]; // Converte para base64
      this.form.patchValue({ file: this.base64File }); // Atualiza o campo 'file' no formulário com o valor base64
    };

    reader.readAsDataURL(file); // Lê o arquivo como Data URL
  }

  // Método que é chamado ao submeter o formulário
  onSubmit() {
    if (this.form.valid) {
      // Obtém os valores do formulário e envia ao back-end
      const formData = this.form.value; // Pega os dados do formulário
      
      // Exemplo de requisição HTTP POST para enviar o formulário
      this.http.post('URL_DO_SEU_BACKEND', formData)
        .subscribe(response => {
          console.log('Formulário enviado com sucesso:', response); // Sucesso
        }, error => {
          console.error('Erro ao enviar o formulário:', error); // Erro
        });
    } else {
      console.error('O formulário é inválido.');
    }
  }
}
