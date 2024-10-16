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
  base64Anexo: string | null = null; // Para armazenar o anexo convertido em base64

  constructor(private fb: FormBuilder, private http: HttpClient) {
    // Cria o formulário com FormBuilder
    this.form = this.fb.group({
      name: [''], // Exemplo de campo de texto
      email: [''], // Exemplo de campo de e-mail
      anexo: ['']   // Campo que será usado para armazenar o anexo em base64
    });
  }

  // Método chamado quando um anexo é selecionado
  onAnexoSelected(event: any) {
    const anexo: File = event.target.files[0]; // Obtém o anexo selecionado

    // Verifica se o anexo é um .msg
    if (anexo && anexo.type === "") {
      this.convertToBase64(anexo); // Converte o anexo para base64
    } else {
      console.error('Por favor, selecione um arquivo .msg válido.');
    }
  }

  // Converte o anexo para base64 e armazena no formulário
  convertToBase64(anexo: File) {
    const reader = new FileReader(); // Usa FileReader para ler o anexo

    reader.onload = () => {
      this.base64Anexo = (reader.result as string).split(',')[1]; // Converte para base64
      this.form.patchValue({ anexo: this.base64Anexo }); // Atualiza o campo 'anexo' no formulário com o valor base64
    };

    reader.readAsDataURL(anexo); // Lê o anexo como Data URL
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
